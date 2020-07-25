``docker build -t jcarpizo:env .``

``docker run -d -v $PWD/webroot/:/var/www/html --name jcarpizo  -p 8090:80 jcarpizo:env``

``docker login``

``docker tag jcarpizo:env jcarpizo/jcarpizo:env``

``docker push jcarpizo/jcarpizo:env``

https://cloud.docker.com/u/jcarpizo/repository/docker/jcarpizo/jcarpizo
