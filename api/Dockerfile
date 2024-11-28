FROM gradle:7 as base

CMD ["bash"]

# build
from base as build
copy . /workspace
run ./gradlew build

# production
from openjdk:17 as production
copy --from=build /workspace/build/libs/api-0.0.1-SNAPSHOT.jar ./api.jar
cmd ["java", "-jar", "api.jar"]