# My notes for Perl client development

## Project preparation

As statet in [README](README.md#getting-started):

```sh
mvn clean package
Downloading: https://repo.maven.apache.org/...
...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary:
[INFO]
[INFO] swagger-codegen-project ............................ SUCCESS [  1.392 s]
[INFO] swagger-codegen (core library) ..................... SUCCESS [ 37.711 s]
[INFO] swagger-codegen (executable) ....................... SUCCESS [ 20.279 s]
[INFO] swagger-codegen (maven-plugin) ..................... SUCCESS [ 33.703 s]
[INFO] swagger-generator .................................. SUCCESS [01:28 min]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 03:01 min
[INFO] Finished at: 2018-09-15T09:44:00+02:00
[INFO] Final Memory: 81M/1271M
[INFO] ------------------------------------------------------------------------
```

## Creating the Perl client package

```sh
java -jar modules/swagger-codegen-cli/target/swagger-codegen-cli.jar generate \
   -i http://petstore.swagger.io/v2/swagger.json \
   -l perl \
   -o /tmp/perl_api_client
```

```sh
find /tmp/perl_api_client/ -type f
```

```
/tmp/perl_api_client/.swagger-codegen-ignore
/tmp/perl_api_client/bin/autodoc
/tmp/perl_api_client/lib/WWW/SwaggerClient/ApiClient.pm
/tmp/perl_api_client/lib/WWW/SwaggerClient/UserApi.pm
/tmp/perl_api_client/lib/WWW/SwaggerClient/Object/Tag.pm
/tmp/perl_api_client/lib/WWW/SwaggerClient/Object/ApiResponse.pm
/tmp/perl_api_client/lib/WWW/SwaggerClient/Object/Pet.pm
/tmp/perl_api_client/lib/WWW/SwaggerClient/Object/Category.pm
/tmp/perl_api_client/lib/WWW/SwaggerClient/Object/Order.pm
/tmp/perl_api_client/lib/WWW/SwaggerClient/Object/User.pm
/tmp/perl_api_client/lib/WWW/SwaggerClient/Configuration.pm
/tmp/perl_api_client/lib/WWW/SwaggerClient/StoreApi.pm
/tmp/perl_api_client/lib/WWW/SwaggerClient/PetApi.pm
/tmp/perl_api_client/lib/WWW/SwaggerClient/Role/AutoDoc.pm
/tmp/perl_api_client/lib/WWW/SwaggerClient/Role.pm
/tmp/perl_api_client/lib/WWW/SwaggerClient/ApiFactory.pm
/tmp/perl_api_client/.gitignore
/tmp/perl_api_client/README.md
/tmp/perl_api_client/.swagger-codegen/VERSION
/tmp/perl_api_client/git_push.sh
/tmp/perl_api_client/docs/Tag.md
/tmp/perl_api_client/docs/User.md
/tmp/perl_api_client/docs/PetApi.md
/tmp/perl_api_client/docs/Category.md
/tmp/perl_api_client/docs/StoreApi.md
/tmp/perl_api_client/docs/UserApi.md
/tmp/perl_api_client/docs/Order.md
/tmp/perl_api_client/docs/ApiResponse.md
/tmp/perl_api_client/docs/Pet.md
/tmp/perl_api_client/t/PetTest.t
/tmp/perl_api_client/t/TagTest.t
/tmp/perl_api_client/t/StoreApiTest.t
/tmp/perl_api_client/t/UserApiTest.t
/tmp/perl_api_client/t/OrderTest.t
/tmp/perl_api_client/t/UserTest.t
/tmp/perl_api_client/t/ApiResponseTest.t
/tmp/perl_api_client/t/CategoryTest.t
/tmp/perl_api_client/t/PetApiTest.t
```

## Running the tests

```sh
cd /tmp/perl_api_client
prove -l
```

This fails when calling without any changes:

```
t/ApiResponseTest.t .. ok
t/CategoryTest.t ..... ok
t/OrderTest.t ........ ok
t/PetApiTest.t ....... 1/1 API Exception(500): Server Error
{"code":500,"type":"unknown","message":"something bad happened"} at lib/WWW/SwaggerClient/PetApi.pm line 106.
# Looks like your test exited with 255 just after 2.
t/PetApiTest.t ....... Dubious, test returned 255 (wstat 65280, 0xff00)
All 1 subtests passed
t/PetTest.t .......... ok
t/StoreApiTest.t ..... 1/1 Use of uninitialized value $_base_value in substitution iterator at lib/WWW/SwaggerClient/StoreApi.pm line 100.
API Exception(405): Method Not Allowed
{"code":405,"type":"unknown"} at lib/WWW/SwaggerClient/StoreApi.pm line 108.
# Looks like your test exited with 255 just after 2.
t/StoreApiTest.t ..... Dubious, test returned 255 (wstat 65280, 0xff00)
All 1 subtests passed
t/TagTest.t .......... ok
t/UserApiTest.t ...... 1/1 API Exception(500): Server Error
{"code":500,"type":"unknown","message":"something bad happened"} at lib/WWW/SwaggerClient/UserApi.pm line 106.
# Looks like your test exited with 255 just after 2.
t/UserApiTest.t ...... Dubious, test returned 255 (wstat 65280, 0xff00)
All 1 subtests passed
t/UserTest.t ......... ok

Test Summary Report
-------------------
t/PetApiTest.t     (Wstat: 65280 Tests: 2 Failed: 1)
  Failed test:  2
  Non-zero exit status: 255
  Parse errors: Bad plan.  You planned 1 tests but ran 2.
t/StoreApiTest.t   (Wstat: 65280 Tests: 2 Failed: 1)
  Failed test:  2
  Non-zero exit status: 255
  Parse errors: Bad plan.  You planned 1 tests but ran 2.
t/UserApiTest.t    (Wstat: 65280 Tests: 2 Failed: 1)
  Failed test:  2
  Non-zero exit status: 255
  Parse errors: Bad plan.  You planned 1 tests but ran 2.
Files=9, Tests=18,  4 wallclock secs ( 0.04 usr  0.02 sys +  2.38 cusr  0.16 csys =  2.60 CPU)
Result: FAIL
```
