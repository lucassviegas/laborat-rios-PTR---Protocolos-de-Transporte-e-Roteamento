# PTR — Protocolos de Transporte e Roteamento (ENE0025)

Repositório com os relatórios dos laboratórios práticos da disciplina **ENE0025 – Protocolos de Transporte e Roteamento**, do curso de Engenharia de Redes de Comunicação da **Universidade de Brasília (UnB)**.

- **Aluno:** Lucas de Souza Viegas
- **Professor:** Prof. Dr. Laerte Peotta de Melo
- **Ambiente prático:** [PNetLab](https://pnetlab.com/), com roteadores Cisco (IOS/IOL)
- **Roteiros oficiais da disciplina:** [github.com/peotta/PTR](https://github.com/peotta/PTR)

## Sobre a disciplina

PTR aborda os fundamentos e a prática de roteamento e transporte em redes de computadores: desde a diferença entre encaminhamento (*forwarding*) e roteamento (*routing*), passando pela configuração de roteadores Cisco (endereçamento IP, SSH, listas de acesso), até protocolos de roteamento dinâmico como **RIPv2** e o roteamento **multicast** com **PIM**. Os laboratórios são realizados em topologias emuladas no PNetLab, seguindo os roteiros publicados pelo professor no repositório [peotta/PTR](https://github.com/peotta/PTR).

## Laboratórios

| # | Título | Tema principal | Status |
| --- | --- | --- | --- |
| 01 | [Configuração inicial no PNetLab](labs/lab01.md) | Diferença entre roteamento e encaminhamento; rota default | ✅ Concluído |
| 02 | [Configuração básica de roteadores no PNetLab](labs/lab02.md) | Configuração inicial de roteador Cisco, SSH, endereçamento IP | ✅ Concluído |
| 03 | [Multicast IP com PIM-DM em topologia controlada](labs/lab03.md) | Roteamento multicast (PIM Dense Mode), IGMP | ⚠️ Parcial — pendência na entrega fim a fim do tráfego multicast |
| 04 | [RIPv2 e Análise de Convergência](labs/lab04.md) | RIPv2, convergência de rede após falha de enlace | ✅ Concluído |

📝 Material de apoio: [Notas de aula — RIP e roteamento dinâmico](labs/notas-aula-rip.md) (inclui gravação da aula)

## Estrutura do repositório

```
.
├── README.md
├── labs/
│   ├── lab01.md
│   ├── lab02.md
│   ├── lab03.md
│   └── lab04.md
└── assets/
    ├── lab01/   # capturas de tela do Laboratório 01
    ├── lab02/   # capturas de tela do Laboratório 02
    ├── lab03/   # capturas de tela do Laboratório 03
    └── lab04/   # capturas de tela do Laboratório 04
```

Cada relatório em `labs/` foi convertido para Markdown a partir dos relatórios originais em `.docx`, mantendo objetivos, topologia, configurações aplicadas, evidências de teste e conclusões de cada laboratório.

## Referência

Os enunciados e roteiros originais dos laboratórios são de autoria do Prof. Dr. Laerte Peotta de Melo e estão disponíveis em [github.com/peotta/PTR](https://github.com/peotta/PTR). Este repositório contém apenas os relatórios de execução produzidos pelo aluno a partir desses roteiros.
