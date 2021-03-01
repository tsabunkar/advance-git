# Bump Version of Project

- **Description**: Bumping/Increasing version of project means-  bumping of 'version' property in package.json file or your  complete project when exported as library (.tgz format)

- Every-time a library is build in CI-CD pipeline, before publish to **Artifactory** like npm we should bump the version of library in-order to say that - Hey new version of this library is available to all the consumers of this library.

- In package.json you will find this property  `"version": "X.Y.Z"` 

  - X :Major Version,

  - Y : Minor Version 

  - Z : Patch version

  - ![img](https://upload.wikimedia.org/wikipedia/commons/thumb/8/82/Semver.jpg/220px-Semver.jpg)

  - NOTE: In general terms this technique is called Software versioning

    - **Software upgrade versioning** is the process of assigning either unique *version names* or unique *version numbers* to unique states of [computer software](https://en.wikipedia.org/wiki/Computer_software). Within a given version number category (major, minor), 

    - These numbers are generally assigned in increasing order and correspond to new developments in the software.

    - Shared libraries in Solaris and [Linux](https://en.wikipedia.org/wiki/Linux) may use the *current.revision.age* format where:

      - *current*: The most recent interface number that the library implements.
      - *revision*: The implementation number of the current interface.
      - *age*: The difference between the newest and oldest interfaces that the library implements

    - 

    - In sequence-based software versioning schemes, each [software release](https://en.wikipedia.org/wiki/Software_release) is assigned a unique identifier that consists of one or more sequences of numbers or letters.

    - Designating development stage:

      

      - |       Stage        | *Alphanumeric suffix* | *Numeric status* | *Numeric 90+* |
        | :----------------: | :-------------------: | :--------------: | :-----------: |
        |       Alpha        |       1.2.0-a.1       |     1.2.0.1      |    1.1.90     |
        |        Beta        |       1.2.0-b.2       |     1.2.1.2      |    1.1.93     |
        | Release candidate  |      1.2.0-rc.3       |     1.2.2.3      |    1.1.97     |
        |      Release       |         1.2.0         |     1.2.3.0      |     1.2.0     |
        | Post-release fixes |         1.2.5         |     1.2.3.5      |     1.2.5     |

- 

