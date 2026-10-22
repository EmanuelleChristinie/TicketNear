<div align="center">

# 🎟️ TicketNear – Event & Stream Radar

**O hub agregador e recomendador inteligente de eventos presenciais e transmissões ao vivo.**

[![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)](#)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](#)
[![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)](#)
[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-222222?style=for-the-badge&logo=GitHub&logoColor=white)](#)

[🚀 Testar Protótipo Interativo Online](#-demonstração-e-protótipo-interativo) • [📌 Sobre o Projeto](#-sobre-o-projeto) • [⚡ Funcionalidades](#-funcionalidades-chave) • [🏗️ Arquitetura](#%EF%B8%8F-arquitetura-e-modelagem-do-sistema) • [🎓 Autoras](#-créditos-e-equipe)

</div>

---

## 📌 Sobre o Projeto

O **TicketNear** foi idealizado para resolver o problema da **fragmentação no ecossistema de entretenimento**. Atualmente, um usuário precisa navegar por $N$ sites e plataformas diferentes para descobrir o que está acontecendo em sua cidade ou onde assistir a uma transmissão oficial.

A plataforma atua como um **radar centralizado e inteligente**, identificando a posição geográfica do usuário, mapeando eventos presenciais em um raio personalizável e direcionando o público para:
- **Bilheterias Oficiais (*Deep Linking*):** Redirecionamento direto e seguro para compra de ingressos.
- **Plataformas de Streaming:** Indicação exata de onde o evento ou jogo será transmitido ao vivo (*Twitch*, *YouTube*, *Prime Video*, *Globoplay*, *Star+*, etc.).

---

## 🚀 Demonstração e Protótipo Interativo

Este repositório possui uma demonstração navegável construída com tecnologias web puras (HTML5, CSS3 com variáveis dinâmicas e JavaScript vanilla) para simular toda a jornada do usuário.

> 🌐 **[Clique aqui para abrir o Protótipo Interativo no GitHub Pages](https://emanuellechristinie.github.io/TicketNear/)**  

### 📱 Recursos do Protótipo
- **Navegação Multitelas:** Alternância instantânea entre radar de eventos, mapa espacial, detalhes da sessão e tela de ingressos.
- **Geolocalização Simulada:** Troca de localização (ex: Brasília, Goiânia, SP) e ajuste dinâmico de raio ($5\text{ km}$, $25\text{ km}$, $50\text{ km}$).
- **Validação de Check-in GPS:** Simulação de raio de proximidade de $200\text{ metros}$ para validação do ingresso via QR Code.

---

## ⚡ Funcionalidades Chave

### 1. 🌐 Geolocalização Dinâmica & Busca por Raio (Geo-Spatial Queries)
- **Raio Personalizável:** Seleção de distância máxima para filtrar atrações ao redor do usuário.
- **Consultas Espaciais:** Suporte a filtros de geofencing com índices espaciais (*PostGIS* / *Redis Geo*).
- **Flexibilidade de Localização:** Chaveamento rápido entre a geolocalização atual via GPS e busca manual por outras cidades ou venues específicas.

### 2. 🔗 Agregador de Bilheteria & Transmissões (*Deep Linking*)
- **Eventos Presenciais:** Encaminhamento com parâmetros rastreáveis para bilheterias parceiras (*Sympla*, *Eventim*, *Ingresso.com*).
- **Transmissões Híbridas/Remotas:** Informação em tempo real sobre os canais oficiais de transmissão (ex: *"Flamengo x Palmeiras — Presencial: Maracanã | Onde assistir: Premiere, Amazon Prime Video"*).
- **Alertas de Escassez:** Notificação de alteração de lotes ou esgotamento iminente na plataforma parceira.

### 3. 🎯 Motor de Recomendação & Radar Personalizado
- **Grafo de Interesses:** Mapeamento de preferências por estilo musical, times de futebol, franquias de cinema e artistas.
- **Radar *O que fazer hoje*:** Feed dinâmico com prioridade para eventos correspondentes ao perfil do usuário ocorrendo nas próximas $24\text{h}$ a $48\text{h}$.

### 4. 🔔 Sistema de Notificações e Mensageria Assíncrona
- **Push & E-mail Geográfico:** Disparo de alertas quando um artista ou time favoritado anunciar apresentação no raio estipulado.
- **Alertas de Transmissão:** Lembrente disparado $15\text{ minutos}$ antes do início da transmissão em streaming.

### 5. 🏆 Gamificação e Check-in Presencial
- **Check-in via GPS:** Liberação do status oficial de presença apenas se o usuário estiver a menos de $200\text{ metros}$ do local do evento.
- **Mural do Evento:** Chat em tempo real e feed exclusivo para os participantes que confirmaram presença ou fizeram check-in presencial.

---

## 🏗️ Arquitetura e Modelagem do Sistema

A arquitetura do **TicketNear** adota um padrão de microsserviços modular e escalável:

```text
┌──────────────────────────────────────────────────────────┐
│                   Cliente Web / Mobile                   │
└────────────────────────────┬─────────────────────────────┘
                             │
                             ▼
┌──────────────────────────────────────────────────────────┐
│              API Gateway (JWT / Auth / Rate Limit)       │
└──────┬─────────────────────┬──────────────────────┬──────┘
       │                     │                      │
       ▼                     ▼                      ▼
┌──────────────┐      ┌──────────────┐      ┌──────────────┐
│  Service:    │      │  Service:    │      │  Service:    │
│  Eventos &   │      │ GeoRadar &   │      │Notificações  │
│ Transmissões │      │ Localização  │      │(Fila / Worker│
└──────┬───────┘      └──────┬───────┘      └──────┬───────┘
       │                     │                      │
       └─────────────────────┼──────────────────────┘
                             ▼
┌──────────────────────────────────────────────────────────┐
│     Banco de Dados Relacional & Espacial (PostGIS/Redis) │
└──────────────────────────────────────────────────────────┘
