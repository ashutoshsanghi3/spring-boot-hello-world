mvn clean install
docker build -t java-spring:latest .
if (docker ps -a | grep 'java-spring')
then
  docker stop java-spring
  docker rm -f java-spring
fi
docker run -d -p 8888:8080 --name java-spring java-spring
mvn test
docker login -u ashutoshsanghi -p dckr_pat_VXlbMIdAp2gqJhEiUOh9J8Ezpv4
docker commit java-spring ashutoshsanghi/java-spring:latest
docker push ashutoshsanghi/java-spring:latest

kubectl delete deploy,svc --all
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
