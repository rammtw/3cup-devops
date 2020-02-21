Имеется готовый образ php-worker/Dockerfile, в нем содержится PHP 7.4

Необходимо внедрить в него библиотеку GDAL.

Для помощи, есть уже готовый образ GDAL на Alpine https://github.com/OSGeo/gdal/blob/master/gdal/docker/alpine-small/Dockerfile

Можно использовать команды из этого Dockerfile.

Примечание:
supervisor конфиги можно не трогать, главное чтобы build контейнера произошел успешно, и внутри него можно было вызвать команды библиотеки GDAL (например ogr2ogr)
