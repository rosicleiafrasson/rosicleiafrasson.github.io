---
layout: post
title: Usar redis com docker
---

#### Baixar a imagem do redis
```docker pull redis```

#### Consultar as imagens disponíveis
```docker images```

#### Iniciar o container a partir da imagem baixada
```docker run --name server-redis -d redis```

#### Exibir containers em execução
```docker ps```

#### Conectar via redis-cli
```docker run -it --link server-redis:redis --rm redis redis-cli -h redis -p 6379```
