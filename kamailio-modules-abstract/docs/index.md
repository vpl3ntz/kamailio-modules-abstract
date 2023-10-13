## Kamailio - Pipelimit module

### How to start

- On Modules Section: 

`loadmodule "pipelimit.so"`


- On setting module-specific parameters:

`modparam("pipelimit", "db_url", "dbdriver://username:password@dbhost:port/dbname")`

----
### Implements validation

- On Routing Logic:

```
request_route {
 $var(variable) = pl_check("pipeid", "algoritmn", "limit");
 switch($var(variable)) {
    case -2:
        xlog("validation -2");
        pl_drop();
        exit;
        break;
    case -1:
        xlog("validation -1");
        pl_drop();
        exit;
        break;
    case 1:
        xlog("validation 1");   
        break;
    case 2:
        xlog("validation 2");   
        break;
    default:
        xlog("default");
        pl_drop();
        exit;
 }
}
```

----
### Exceptions

If you change the default names, when change default value:

##### Table
Name default: `pl_pipes`, to change:

`modparam("pipelimit", "plp_table_name", "NAME")`

##### Pipeid column
Name default: `pipeid`, to change:

`modparam("pipelimit", "plp_pipeid_column", "NAME")`
##### Limit column
Name default: `limit`, to change:

`modparam("pipelimit", "plp_limit_column", "NAME")`

##### Algorithm column
Name default: `algorithm`, to change:

`modparam("pipelimit", "plp_algorithm_column", "NAME")`

----
### Algorithms

Here is someone it's tasted.

##### Tail Drop (TAILDROP)

`pl_check("name", "TAILDROP", "3");`

***Obs: In tests, was verificated third parameter called "limit" has the max limit number 3, after this,  at the start of each interval an internal counter is reset and incremented for each incoming message.***

##### Random Early Detection (RED)

`pl_check("name", "RED", "limit");`

***Obs: In testes, was verificated the algorithms doesn't validate "limit", it was random.***