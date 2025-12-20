## SPRINT 1 — Criação do benchmark (início)

O foco foi iniciar a **criação de um benchmark** para comparar os principais algoritmos de criptografia pós-quântica (PQC) voltados a assinatura digital, reunindo os primeiros parâmetros de comparação e organizando a base para testes e tabelas comparativas.

---

## SPRINT 2 — Criação do benchmark (consolidação)

Consolidação do benchmark com a comparação de algoritmos e variantes (ex.: **Falcon**, **Dilithium**, **SPHINCS+**), priorizando métricas essenciais para uso em blockchain:

- **Tamanho de chave pública/privada**
- **Nível de segurança** (ex.: níveis 1, 3 e 5)
- **Tamanho da assinatura**

O objetivo foi produzir um comparativo claro para **embasar a escolha do algoritmo** mais viável para uma implementação futura na Solana.

---

## SPRINT 3 — Criação de um programa on-chain (arquitetura híbrida)

Implementação de uma **arquitetura híbrida (off-chain + on-chain)**:

- **Off-chain**: geração do par de chaves PQC (pública/privada), com a **chave privada** mantida localmente pelo usuário e a **chave pública** enviada/armazenada em um **PDA**.
- **On-chain**: tentativa de realizar a **verificação de assinaturas PQC** dentro do programa.

Resultado: a execução não funcionou como esperado devido a **restrições do ambiente on-chain da Solana (BPF/SVM)**, que inviabilizaram a verificação PQC em um programa convencional.

---

## SPRINT 4 — Tentativa de criação de syscall nativo (verificação PQC no runtime)

Exploração e proposta de uma alternativa mais aderente às limitações do ambiente on-chain: criação de um **syscall nativo** no runtime da Solana para executar a verificação PQC de forma nativa, evitando as restrições típicas de programas BPF.

O objetivo foi viabilizar a verificação de assinaturas PQC on-chain com maior compatibilidade e desempenho, preparando o caminho para uma transição híbrida.

---

## SPRINT 5 — Artigo e consolidação da proposta de transição PQC na Solana

Consolidação do trabalho no formato de artigo, com foco na **viabilidade técnica** da transição para criptografia pós-quântica na Solana, combinando duas frentes:

- **Programa híbrido**: autenticação clássica (Ed25519) combinada com **verificação PQC (Falcon-512)** em uma arquitetura em duas camadas (gestão de chaves off-chain e verificação on-chain).
- **Proposta de syscall nativo** (ex.: `sol_falcon512_verify`): modificação do runtime para permitir verificação PQC de forma eficiente e compatível com as restrições de execução on-chain.

Também foram discutidos os principais desafios: necessidade de mudança no runtime/feature flag, limitações do BPF para algoritmos baseados em reticulados e impactos de custo/armazenamento devido ao aumento de tamanhos de chaves e assinaturas.


