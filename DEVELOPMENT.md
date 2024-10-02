Use this command to run tests locally:

```sh
docker-compose -f docker-compose.yml up -d && docker-compose -f docker-compose-extra.yml  build --build-arg CHECK_CODE=false --build-arg PG_VERSION=14 tests && docker-compose -f docker-compose-extra.yml run tests
```

To debug changes in the code use `elog` in the code, they'll be printed in the output of the tests.

To get the results of the tests locally change `docker-compose-extra.yml` to connect a volume

```
volumes:
    - /tmp/pg:/tmp
```

Use `/tmp` as the outputdir when running the tests by changing this line in `CMakeLists.txt`:

```sh
add_custom_target(installcheck
	COMMAND ${PG_REGRESS} --inputdir=${PROJECT_SOURCE_DIR}/tests --outputdir=/tmp/tests \${EXTRA_REGRESS_OPTS} ${REGRESS_TESTS})
```

Now you can get the results file from `/tmp/pg/tests/results` and override the `.out` files with the actual output expected.
