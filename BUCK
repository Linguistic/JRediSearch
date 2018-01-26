include_defs("//DEFS")

#################################
#      Remote Dependencies      #
#################################
remote_jar(
    name="commons-pool2",
    url="mvn:org.apache.commons:commons-pool2:jar:2.5.0",
    hash="cb7d05e6308ad795decc4a12ede671113c11dd98"
)

remote_jar(
    name="gson",
    url="mvn:com.google.code.gson:gson:jar:2.8.2",
    hash="3edcfe49d2c6053a70a2a47e4e1c2f94998a49cf"
)

remote_jar(
    name="hamcrest",
    url="mvn:org.hamcrest:hamcrest-core:jar:1.3",
    hash="42a25dc3219429f0e5d060061f71acb49bf010a0"
)

remote_jar(
    name="junit",
    url="mvn:junit:junit:jar:4.12",
    hash="2973d150c0dc1fefe998f834810d68f278ea58ec"
)

################################
#      Local Dependencies      #
################################
# TODO: replace this with an actual release after jedis#1576 is resolved
prebuilt_jar(
    name="jedis",
    binary_jar="./jedis-3.0.0.jar"
)

############################
#      Actual Library      #
############################
deps = ['//:commons-pool2', '//:jedis', '//:gson']

java_test(
    name="redisearch-tests",
    srcs=glob(["src/test/**/*.java"]),
    deps=['//:redisearch-lib', '//:hamcrest', '//:junit'] + deps
)

java_library(
    name="redisearch-lib",
    srcs=glob(["src/main/**/*.java"]),
    deps=deps
)

java_binary(
    name="redisearch-1.0",
    deps=["//:redisearch-lib"],
    tests=['//:redisearch-tests']
)