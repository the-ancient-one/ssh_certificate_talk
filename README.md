# ssh_certificate_talk
Talk/Session for warwick BSc year 2. 

$${\color{red}(Not \space recommended \space for \space production \space use \space only \space for \space testing/demo!)}$$ 

## File details 

| File path | Comment |  
|----------|----------| 
|  Dockerfile_client |   Dockerfile for the ssh client  |  
|  Dockerfile_server  |  Dockerfile for the ssh destination and CA server  |   
|  docker-compose.yml |  Docker Compose file for creating containers and network  |   
|  docker-entrypoint.sh |  startup file for the ssh server to start ssh daemon services   |   
|  slides/SSH Talk (Jumphost DMZ System).pdf |  Slides used in the presentation  |   

## Repo Structure 
```
.
├── Dockerfile_client
├── Dockerfile_server
├── LICENSE
├── README.md
├── docker-compose.yml
├── docker-entrypoint.sh
└── slides
    └── SSH Talk (Jumphost DMZ System).pdf
```

## Configuration and Testing SSH 

In this we will be using default configurations and file locations, this is not recommended for production. 
- Login into to containers.  `docker exec -it ssh_server /bin/bash`
- Create CA key pair `ssh-keygen -t rsa -f /etc/ssh/ca `
- Create client key pair `ssh-keygen -t rsa `
- Copy the public key into CA server `cat ~/.ssh/id_rsa.pub ` and save as `bob.pub` in the same location as CA cert (/etc/ssh/ca).
- Change directory `cd /etc/ssh/` and use the command `ssh-keygen -s ca -I bob -n apache -V +1d -z +1 -O no-x11-forwarding  -O no-agent-forwarding -O no-port-forwarding bob.pub`to sign and generate certificate.
- Verify using `ssh-keygen -Lf bob-cert.pub`.
- Copy `cat bob-cert.pub` to client `vi ~/.ssh/id_rsa-cert.pub`.
- Copy `cat /etc/ssh/ca.pub` to client `echo "@cert-authority * "remove quote and replace with cat ca.pub" " >> ~/.ssh/known_hosts`. 
- From client `ssh apache@172.20.0.2` work fine now. 