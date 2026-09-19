# Notas de aula — RIP e roteamento dinâmico

> Disciplina: ENE0025 - Protocolos de Transporte e Roteamento (UnB) · Prof. Dr. Laerte Peotta de Melo
> Roteiro de referência do laboratório: [peotta/PTR](https://github.com/peotta/PTR)
> [⬅ Voltar ao README](../README.md) · [Ver relatório do Laboratório 04](lab04.md)

🎧 [Gravação da aula](../assets/lab04/audio/aula-rip.m4a)

---

## Roteamento dinâmico e convergência
Em roteamento dinâmico, não é necessário reconfigurar manualmente todos os caminhos quando um roteador anuncia uma nova rota ou quando ocorre mudança de topologia. A rede reage a alterações como inserção de roteadores, queda de enlaces e mudanças nos caminhos disponíveis.
A analogia usada foi o **Waze/GPS**: quando uma via está fechada ou há acidente, o sistema pode recalcular a rota. Essa mudança não é instantânea; existe um tempo até que a nova informação se propague e a rota seja alterada. Em redes, esse intervalo é o **tempo de convergência do roteamento**.
A escolha de rota depende do protocolo e da métrica usada. A menor distância nem sempre é o melhor caminho: uma rota mais curta pode passar por um enlace de **512 Kb/s**, enquanto uma rota mais longa pode usar um enlace de **10 Gb/s**. No caso do **RIP**, a métrica considerada é apenas o número de saltos, não,, congestionamento ou qualidade do enlace.
## IGP, EGP, AS e BGP
Os protocolos de roteamento foram divididos em dois tipos principais:
- **IGP — Interior Gateway Protocol**: funciona dentro de uma única rede ou sistema autônomo.
- **EGP — Exterior Gateway Protocol**: interliga redes diferentes, especialmente entre sistemas autônomos na Internet.
Um **AS** ou **ASN** representa um sistema autônomo. A **UNB**, por exemplo, pode ser vista como uma rede com vários roteadores dentro de um AS. Na borda dessa rede, normalmente há um protocolo como o **BGP** para interligação com outras redes, como a **RNP**, provedores, empresas, Google e demais partes da Internet.
O **RIP** é um **IGP**, ou seja, um protocolo interno. Ele não é usado para interligar grandes redes na Internet. Para comunicação entre sistemas autônomos, o protocolo típico é o **BGP**, um protocolo de borda.
## Funcionamento do RIP
O **RIP — Routing Information Protocol** funciona como uma “fofoca” entre roteadores. Cada roteador anuncia periodicamente aos seus vizinhos as redes que conhece. Os vizinhos acreditam nessa informação, atualizam suas tabelas de roteamento e repassam as informações adiante.
Esse modelo exige confiança entre roteadores. Por isso, em versões posteriores, mecanismos de autenticação foram introduzidos para evitar que um roteador malicioso anuncie rotas falsas.
Principais características do RIP:
- Usa **vetor de distância**.
- Considera apenas o número de **saltos** até o destino.
- Cada salto tem custo **1**.
- Cada roteador soma **1** à métrica recebida do vizinho antes de repassar a rota.
- É fácil de configurar, fácil de manter e tem baixo custo computacional.
- Funciona bem em redes pequenas e ambientes legados.
- Aprende redes automaticamente, sem necessidade de configuração manual de todas as rotas.
No RIP, cada destino fica associado a um único melhor caminho na tabela de roteamento. Se uma rota cair, a rede refaz o caminho, mas não mantém várias rotas equivalentes para o mesmo destino como alternativas simultâneas.
## Vetor de distância e Bellman-Ford
O RIP é baseado em vetor de distância e tem relação com o algoritmo **Bellman-Ford**, criado nos anos **50**. O objetivo é calcular o custo mínimo entre vértices de um grafo.
No Bellman-Ford original, os custos das arestas podem variar e até assumir valores negativos. No RIP, a métrica é simplificada: cada enlace vale sempre **1 salto**. Portanto, o protocolo só conta quantos roteadores precisam ser atravessados até o destino.
Exemplo conceitual:
- Um roteador diretamente conectado a uma rede tem custo **0** para essa rede.
- Um vizinho que aprende essa rota soma **1**, ficando com custo **1**.
- O próximo roteador soma mais **1**, ficando com custo **2**.
Assim, a rota é propagada de vizinho em vizinho, sempre acumulando saltos.
## Limitação de 15 saltos
No RIP, a métrica máxima válida é **15 saltos**. A métrica **16** representa infinito, ou seja, rota inalcançável.
Quando uma rota deixa de responder, os roteadores vão aumentando a métrica até chegar a **16**. Nesse ponto, a rota é considerada inalcançável e removida da tabela. Essa limitação surgiu por decisão de engenharia em uma época em que as redes eram pequenas, com poucos roteadores.
Hoje, essa limitação não atende bem a redes grandes, nas quais o caminho pode atravessar dezenas ou centenas de roteadores. Por isso, o RIP é mais adequado para redes pequenas e tem uso principalmente didático ou em ambientes legados.
## Histórico do RIP
O histórico apresentado mostra que o RIP não surgiu do nada, mas de uma evolução de pesquisas e necessidades práticas:
- **Anos 50**: surgimento das ideias de vetor de distância e cálculo de menor caminho associadas ao **Bellman-Ford**.
- **Anos 70**: a **Xerox** aplicou conceitos semelhantes no protocolo **PUP**.
- **1982**: o **BSD Unix** passou a usar o protocolo em seu sistema operacional.
- **1988**: a **RFC 1058** documentou o **RIPv1**.
- Posteriormente, a **RFC 2453** trouxe o **RIPv2**.
O **RIPv1** era classful, não trabalhava com máscara de rede variável e não suportava **CIDR/VLSM**. Isso gerava limitações importantes, especialmente em redes grandes, porque o protocolo propagava rotas sem máscara adequada e usava broadcast.
O **RIPv2** trouxe melhorias importantes:
- Suporte a **VLSM/CIDR**.
- Uso de **multicast** em vez de broadcast.
- Introdução de autenticação.
- Manutenção da lógica de vetor de distância.
Apesar dessas melhorias, o RIP continuou limitado pela métrica de saltos e pelo máximo de **15 saltos**.
## RIP em comparação com outros protocolos
O RIP ainda pode aparecer em redes legadas ou em ambientes pequenos, porque é simples, rápido de implementar e exige pouco processamento.
Em redes corporativas e de maior escala, protocolos como **OSPF** e **IS-IS** são mais comuns. O **IS-IS** aparece bastante em ambientes de grande escala e data centers. Já o **BGP** é usado como protocolo externo, principalmente para comunicação entre sistemas autônomos.
No laboratório da disciplina, o foco será observar protocolos internos e externos, passando por **RIP**, **OSPF** e **BGP**.
## Atualizações, multicast e UDP
O RIP envia periodicamente atualizações da tabela de roteamento para os vizinhos.
Características citadas:
- As atualizações ocorrem a cada **30 segundos**.
- No **RIPv2**, os anúncios usam multicast.
- O endereço multicast padrão é **224.0.0.9**.
- O transporte usado é **UDP**.
- A porta convencional é **520**.
Nem todos os protocolos de roteamento usam UDP ou TCP. O **OSPF**, por exemplo, usa diretamente a camada 3 para difusão.
O envio periódico é simples, mas gera tráfego desnecessário, porque a tabela pode ser reenviada mesmo quando nada mudou.
## Temporizadores do RIP
Os principais temporizadores apresentados foram:
- **30 segundos**: intervalo entre anúncios periódicos.
- **180 segundos**: tempo para considerar uma rota inválida se não houver resposta.
- **180 segundos**: período de hold-down, usado como segurança quando a origem da rota não pode ser confirmada.
- **240 segundos**: tempo de flush, quando a rota é removida da tabela.
Quando uma falha ocorre, a informação precisa ser detectada, propagada e recalculada pelos roteadores. Enquanto isso não acontece, a rede pode continuar usando informações antigas. A convergência só termina quando todos os roteadores passam a ter uma visão consistente da topologia.
## Problemas do RIP e técnicas de melhoria
O RIP pode sofrer com demora na convergência e com loops de roteamento. Como as atualizações são periódicas e a métrica vai aumentando gradualmente até **16**, a rede pode levar muito tempo para concluir que uma rota está inalcançável.
Técnicas citadas para reduzir esses problemas:
### Split horizon
Uma rota aprendida por uma interface não deve ser anunciada de volta pela mesma interface. Isso evita que um roteador aceite como melhor caminho uma rota que ele próprio ajudou a divulgar, reduzindo risco de loop.
### Route poisoning
Quando uma rota deixa de existir, ela pode ser anunciada imediatamente com métrica **16**, indicando que está inalcançável. Isso antecipa a remoção da rota, sem esperar a contagem gradual até o infinito.
### Triggered update
Quando ocorre uma mudança, a atualização pode ser enviada imediatamente, sem esperar o ciclo periódico de **30 segundos**. Isso acelera a convergência.
Essas técnicas são adaptações para contornar limitações do RIP, principalmente em relação a loops e tempo de convergência.
## Laboratório proposto
O laboratório usará uma topologia com **3 roteadores**. Os roteadores podem ser **Cisco** ou outros, conforme disponibilidade. A recomendação é começar a usar **Linux** em vez de apenas **VPC**, porque o Linux se aproxima mais de cenários reais.
A topologia inclui roteadores com interfaces **in** e **out**, redes distintas e enlaces entre roteadores. O cabeamento usado será **gigabit**, não serial.
Foram citadas redes de classes diferentes e sub-redes com **/24** para algumas LANs. Para enlaces ponto a ponto entre roteadores, foi usado **/30**, porque uma rede /30 fornece **4 endereços IP** no total: um endereço de rede, um endereço de broadcast e **2 IPs válidos** para hosts. Esse uso é comum em conexões diretas entre roteadores.
A proposta do laboratório inclui:
- Configurar RIP nos roteadores.
- Verificar se o protocolo está funcionando.
- Conferir se os roteadores estão conectados e com as interfaces ativas.
- Observar a troca de tabelas de roteamento.
- Usar **ping** para testar comunicação entre máquinas passando pelos roteadores.
- Usar **Wireshark** ou uma imagem Docker com Wireshark para capturar o tráfego RIP.
- Verificar o uso de **UDP**, porta **520** e multicast **224.0.0.9**.
- Derrubar uma ligação, como entre **R2** e **R3**, para observar a convergência.
- Medir quanto tempo o **R1** leva para passar a se comunicar diretamente com o **R3** quando a rota via **R2** deixa de funcionar.
O objetivo principal do laboratório é medir a convergência e visualizar, na prática, como o RIP propaga informações de roteamento e reage a falhas.