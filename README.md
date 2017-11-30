# maven-nodejs-proxy

This role installs the wcm.io [Maven NodeJS
Proxy](https://github.com/wcm-io-devops/maven-nodejs-proxy).

## Requirements

This role requires

* Ansible 2.0 or higher
* Maven 3.3.9 or higher
* Git
* JDK 1.8

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yml
maven_nodejs_proxy_maven_cmd: mvn
```

The maven command to execute.

```yml
maven_nodejs_proxy_maven_opts: "-B -U"
```

The command line options for maven.

```yml
maven_nodejs_proxy_repo: https://github.com/wcm-io-devops/maven-nodejs-proxy.git
```

The git repository to clone

```yml
maven_nodejs_proxy_force_compile: false
```

Controls if maven-nodejs-proxy is compiled on every run. When set to
false the maven compile step will be skipped when the jar already exists

```yml
maven_nodejs_proxy_version: "1.1.0"
```

The version (Commit,Branch,Tag) to checkout and compile.

```yml
maven_nodejs_proxy_user: 'maven-nodejs-proxy'
```

The system user for the service.

```yml
maven_nodejs_proxy_group: 'maven-nodejs-proxy'
```

The system user group for the service.

```yml
maven_nodejs_proxy_binary: "io.wcm.devops.maven.nodejs-proxy-{{ maven_nodejs_proxy_version }}.jar"
```

The name of the compiled jar.

```yml
maven_nodejs_proxy_home: /opt/srv/maven-nodejs-proxy
```

The home directory of the service.

```yml
maven_nodejs_proxy_binary_path: "{{ maven_nodejs_proxy_home }}/{{ maven_nodejs_proxy_binary }}"
```

The path to the jar.

```yml
maven_nodejs_proxy_config_path: "{{ maven_nodejs_proxy_home }}/config.yml"
```

The path to the config.yml.

```yml
maven_nodejs_proxy_tmp: /tmp/nodejs-proxy
```

Path to the tmp directory (used for git clone and compile).

```yml
maven_nodejs_proxy_tmp_binary_path: "{{ maven_nodejs_proxy_tmp }}/maven-nodejs-proxy/target/{{ maven_nodejs_proxy_binary }}"
```

The path to the jar file in the tmp directory.

```yml
maven_nodejs_proxy_port: 8080
```

The http port under which the server will run

```yml
maven_nodejs_proxy_log_dir: /var/log/maven-nodejs-proxy
```

Log file directory.

```yml
maven_nodejs_proxy_log_file: "{{ maven_nodejs_proxy_log_dir }}/maven-nodejs-proxy.log"
```

Path to the logfile.

```yml
maven_nodejs_proxy_log_file_archive: "{{ maven_nodejs_proxy_log_dir }}/maven-nodejs-proxy-%d.log"
```

Pattern for the archived log files (log rotation).

```yml
maven_nodejs_proxy_service_name: maven-nodejs-proxy
```

The name of the service.

```yml
maven_nodejs_proxy_service_args: "java -jar {{ maven_nodejs_proxy_binary_path }} server {{ maven_nodejs_proxy_config_path }}"
```

The arguments for the service.

```yml
maven_nodejs_proxy_config:
  groupId: org.nodejs.dist
  nodeJsArtifactId: nodejs-binaries
  npmArtifactId: npm-binaries
  nodeJsBinariesRootUrl: "https://nodejs.org/dist"
  nodeJsBinariesUrl: "/v${version}/node-v${version}-${os}-${arch}.${type}"
  nodeJsBinariesUrlWindows: "/v${version}/win-${arch}/node.${type}"
  nodeJsBinariesUrlWindowsX86Legacy: "/v${version}/node.${type}"
  nodeJsBinariesUrlWindowsX64Legacy: "/v${version}/${arch}/node.${type}"
  npmBinariesUrl: "/npm/npm-${version}.${type}"
  nodeJsChecksumUrl: "/v${version}/SHASUMS256.txt"
  httpClient:
    connectionTimeout: 2s
    timeout: 5s
    timeToLive: 1h
    cookiesEnabled: false
    retries: 2
    userAgent: Maven NodeJS Proxy
  server:
    gzip:
      enabled: false
  logging:
    level: INFO
```

The configuration used for the
[config.yml.j2](templates/config.yml.j2) template.

## Dependencies

The role has no hard coded depencencies but works well together with

* geerlingguy.java
* role: gantsign.maven
* geerlingguy.git

## Example Playbook

    - name: Setup maven-nodejs-proxy
      hosts: maven-nodejs-proxy
      vars:
        java_packages:
          - openjdk-8-jdk
    
      roles:
        - role: geerlingguy.java
        - role: gantsign.maven
        - role: geerlingguy.git
        - role: maven-nodejs-proxy

## License

Apache 2.0
