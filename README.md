# kyte-mysql

MySQL/MariaDB driver for Kyte on the async runtime. A Kyte package - fetch with:

```sh
kyte get https://github.com/kytelang/kyte-mysql
```

```kyte
import mysql;
```

## Design

Split by responsibility; consumers only touch the driver interface (`MyDriver` / `MyConnection`).

| Module       | Responsibility                                                                       |
| ------------ | ------------------------------------------------------------------------------------ |
| `mysql`      | Driver interface: `MyConnection impl Connection` + `MyDriver impl Driver` + connect/handshake.   |
| `connection` | Connection-string parsing (`ConnectionOptions`, `parse`).                            |
| `codec`      | Wire codec - packet framing, builders, decoders (incl. binary protocol), `MyCursor`. |
| `typemap`    | MySQL type→`DbType`, `DbValue`→SQL/bind text, `substituteParams`.                    |
| `proto`      | Async transport framing (`MyReader`, `Packet`, `readPacket`, `sendPacket`).          |
| `stmt`       | Prepared-statement cache entry (`MyStmt`).                                           |
| `auth`       | Auth scrambles (native / sha2 / caching_sha2 / RSA) + capability flags.              |
