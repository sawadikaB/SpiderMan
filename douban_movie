# -*-coding:utf-8 -*-
import requests
from HTMLParser import HTMLParser

class DouBanMovie(object):

    def __init__(self):
        object.__init__(self)
        headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) '
                                'Chrome/49.0.2623.221 Safari/537.36 SE 2.X MetaSr 1.0'}
        self.headers = headers
        self.movies = []

    def getpagecode(self):
        url = 'https://movie.douban.com/cinema/nowplaying/chengdu/'
        res = requests.get(url, headers=self.headers)
        food = res.content
        shit = _getmovies(food, self.movies)

def _getmovies(food, movies):

        def _attr(attrlist, attrname):
            for attr in attrlist:
                if attr[0] == attrname:
                    return attr[1]
            return None

        class MovieHTMLParser(HTMLParser):

            def __init__(self):
                HTMLParser.__init__(self)
                self.in_movie = False

            def handle_starttag(self, tag, attrs):

                if tag == 'li'  and _attr(attrs, 'data-category') == 'nowplaying':
                    movie = {}
                    movie['data-title'] = _attr(attrs, 'data-title')
                    movie['data-score'] = _attr(attrs, 'data-score')
                    movie['data-region'] = _attr(attrs, 'data-region')
                    movie['data-director'] = _attr(attrs, 'data-director')
                    movie['data-actors'] = _attr(attrs, 'data-actors')
                    movies.append(movie)
                    self.in_movie = True


                if self.in_movie and tag == 'img':
                    self.in_movie = False
                    movie = movies[len(movies)-1]
                    movie['pic-src'] = _attr(attrs, 'src')
                    _down_pic(movie)
                    print '电影名:%(data-title)s|评分:%(data-score)s|国家:%(data-region)s|导演:%(data-director)s|' \
                          '演员:%(data-actors)s|文件名称:%(filename)s|' % movie

        def _down_pic(movie):
            src = movie['pic-src']
            piccontent = requests.get(src).content
            filename = src.split('/')[-1]
            with open(filename, 'wb') as f:
                f.write(piccontent)
            movie['filename'] = filename
            return movie




        i = MovieHTMLParser()
        i.feed(food)
        return movies



if __name__ == '__main__':
    g = DouBanMovie()
    g.getpagecode()
    


