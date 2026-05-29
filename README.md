<p align="center">
  <img src="assets/img/logo_inteli.png" width="180">
</p>

## Projeto: Proposta de Transição para Criptografia Pós-Quântica na Solana

**Trilha Acadêmica** | Instituto de Tecnologia e Liderança
**Trabalho de Conclusão de Curso**

* **Autor:** Arthur Tsukamoto
* **Curso:** Ciência da Computação
* **Orientadora:** Profa. Dra. Ana Cristina Santos
* **Área:** Criptografia Pós-Quântica, Blockchain, Sistemas Distribuídos

---

## Sobre o Projeto

Este projeto foi desenvolvido no âmbito da Trilha Acadêmica do Inteli como parte do Trabalho de Conclusão de Curso.

O estudo investiga a viabilidade da integração do algoritmo pós-quântico Falcon-512 à blockchain Solana, analisando as limitações e restrições arquiteturais do runtime da rede e contribuindo com uma proposta híbrida (on-chain / off-chain). A proposta consiste na criação de um programa que utiliza um *syscall* nativo para verificação de assinaturas Falcon-512 por meio da biblioteca `pqcrypto`. O *syscall* foi implementado em um fork do `solana-test-validator`, modificado para permitir o uso da biblioteca `pqcrypto` e realizar a verificação on-chain.

---

## Problema de Pesquisa

Computadores quânticos criptograficamente relevantes ameaçam esquemas de assinatura digital baseados em fatoração de inteiros e logaritmos discretos, incluindo o Ed25519, esquema que sustenta a autenticação de transações na Solana. Esse cenário viabiliza estratégias adversariais do tipo *Harvest Now, Decrypt Later*, nas quais dados capturados hoje podem ser explorados quando a capacidade quântica estiver disponível.

A migração para criptografia pós-quântica na Solana, no entanto, esbarra em restrições arquiteturais relevantes: o limite de 1.232 bytes por transação penaliza esquemas com chaves e assinaturas grandes, e a Solana Virtual Machine (SVM), derivada do modelo eBPF/SBF, não suporta nativamente as operações de ponto flutuante exigidas pela verificação do Falcon-512.

Diante desse contexto, a pergunta de pesquisa é: **é viável integrar algoritmos pós-quânticos de assinatura digital à Solana de forma nativa e eficiente, preservando segurança e desempenho?**

---

## Objetivo

Avaliar tecnicamente a viabilidade de uma transição pós-quântica híbrida na Solana, em vez de uma migração completa da rede, por meio de:

* Análise comparativa de algoritmos pós-quânticos quanto à compatibilidade com o limite de 1.232 bytes por transação da Solana.
* Implementação de uma arquitetura híbrida que combina Ed25519 (compatibilidade nativa) com Falcon-512 (camada adicional de autenticação pós-quântica).
* Vinculação da chave pública pós-quântica à identidade Ed25519 do usuário por meio de uma *Program Derived Address* (PDA).
* Extensão experimental do *runtime* do validador via um *syscall* nativo (`sol_falcon512_verify`) para verificação de assinaturas Falcon-512.
* Caracterização dos efeitos da abordagem sobre tempo de assinatura, tempo de confirmação, tamanho de transação e consumo de *Compute Units*.

---

## Abordagem Metodológica

O estudo foi conduzido em duas etapas complementares.

### Etapa 1: Arquitetura Híbrida com Verificação On-Chain

Implementação de uma arquitetura híbrida com geração *off-chain* do par de chaves Falcon-512, registro da chave pública em uma PDA derivada da identidade Ed25519 do usuário e submissão de transações com dupla autenticação. Essa etapa verificou experimentalmente a inviabilidade da verificação Falcon-512 diretamente na SVM, em decorrência da dependência do algoritmo da aritmética de ponto flutuante de dupla precisão (IEEE 754) para amostragem gaussiana sobre reticulados NTRU.

### Etapa 2: Extensão do Runtime via Syscall Nativo

A verificação criptográfica foi deslocada para o *runtime* do validador por meio do *syscall* nativo `sol_falcon512_verify`, implementado em um fork do Agave (repositório principal da Solana). O *syscall* realiza o mapeamento de memória da SVM, valida estruturalmente os componentes criptográficos e delega a verificação à biblioteca `pqcrypto_falcon`.

### Benchmark Comparativo

Foram avaliadas **20.360 transações**, distribuídas em lotes de N igual a 5, 25, 50, 100 e 10.000, comparando três cenários:

1. Transferência nativa autenticada por Ed25519.
2. Transferência autorizada por Falcon-512 verificada via *syscall* nativo.
3. Cenário analítico de Ed25519 sob a mesma arquitetura do Falcon-512 (PDA combinado com *syscall* hipotético), derivado das constantes oficiais do `execution_budget.rs`.

---

## Resultados Principais

* **Taxa de sucesso de 100%** em todas as execuções, com métricas estáveis em todos os lotes.
* **Tempo de assinatura:** Falcon-512 (148,7 µs) foi aproximadamente 4,7× maior que o Ed25519 (31,9 µs), mas permaneceu na ordem de microssegundos.
* **Tamanho de transação:** 934 bytes para o fluxo Falcon-512, contra 215 bytes do Ed25519 nativo, permanecendo dentro do limite de 1.232 bytes da Solana.
* **Tempo de confirmação:** diferença inferior a 1% entre os cenários (529,0 ms contra 523,8 ms).
* **Compute Units:** 9.614 CU para o fluxo Falcon-512, dos quais apenas 900 CU (9,4%) correspondem à verificação criptográfica via *syscall*. O restante é overhead arquitetural (leitura de PDA, desserialização da chave pública de 897 bytes, `find_program_address` etc.).
* **Paridade técnica arquitetural:** o Ed25519 hipotético sob a mesma arquitetura consumiria aproximadamente 8.834 CU, indicando paridade (1,09×) com o Falcon-512 medido (9.614 CU). O custo adicional do fluxo pós-quântico tem origem arquitetural, **não criptográfica**.
* **Conclusão prática:** a verificação Falcon-512 diretamente na SVM é inviável, enquanto a verificação no *runtime* via *syscall* nativo é operacionalmente viável.

---

## Documento Científico

O artigo científico consolidado e a apresentação final estão disponíveis em:

* `Modulo_4/Viabilidade_de_uma_Transição_Pós_Quântica_Híbrida_na_Blockchain_Solana_via_Syscall_Nativo.pdf`: artigo submetido ao Congresso Brasileiro de Ciências e Tecnologias Quânticas (CBCTQ 2026).
* `Modulo_4/TCC.pdf`: apresentação final do Trabalho de Conclusão de Curso.
* `Modulo_1/Arthur_Tsukamoto_public_report.pdf`: relatório técnico público (PT_BR).
* `Modulo_1/Arthur_Tsukamoto_public_report_en.pdf`: relatório técnico público (EN).

---

## Tecnologias Utilizadas

* **Rust:** linguagem do *runtime* da Solana e do programa on-chain.
* **Solana / Agave:** fork do validador modificado com a inclusão do *syscall* nativo `sol_falcon512_verify`.
* **`solana_test_validator`:** ambiente *single-node* utilizado nos experimentos.
* **SVM (Solana Virtual Machine):** ambiente de execução baseado em eBPF/SBF.
* **Program Derived Addresses (PDAs):** armazenamento persistente da chave pública Falcon-512 (layout determinístico de 946 bytes).
* **`pqcrypto_falcon`:** biblioteca Rust utilizada na verificação Falcon-512 a partir do *syscall* nativo.
* **Falcon-512:** esquema de assinatura pós-quântica baseado em reticulados NTRU.
* **Ed25519:** esquema clássico utilizado nativamente pela Solana, mantido como camada de compatibilidade.
* **Cargo / Toolchain Rust:** *build*, dependências e execução de benchmarks.

---

## Evolução do Projeto entre Módulos

O projeto foi desenvolvido de forma incremental ao longo de quatro módulos, partindo de uma fundamentação teórica até a validação experimental do mecanismo proposto.

### Módulo 1: Fundamentação Teórica e Análise Comparativa

Levantamento da arquitetura criptográfica da Solana (Ed25519, Proof of History, Tower BFT) e revisão dos algoritmos pós-quânticos padronizados pelo NIST em 2024 (ML-KEM/FIPS 203, ML-DSA/FIPS 204, Falcon e SLH-DSA/FIPS 205). Análise dos fundamentos matemáticos (reticulados estruturados, NTRU, framework Fiat-Shamir, esquemas baseados em hash) e dos trade-offs de tamanho entre os algoritmos. Conclusão de que o Falcon-512 é o candidato mais promissor para a Solana, por oferecer o menor *overhead* de tamanho (16,3×) frente ao Ed25519 entre os esquemas pós-quânticos avaliados.

### Módulo 2: Benchmark e Primeiras Implementações

Desenvolvimento iterativo em cinco sprints:

* **Sprints 1 e 2:** consolidação de um benchmark comparativo de algoritmos PQC (Falcon, Dilithium, SPHINCS+) com foco em tamanho de chaves, nível de segurança e tamanho de assinatura, embasando a escolha do algoritmo para Solana.
* **Sprint 3:** primeira implementação de uma arquitetura híbrida (gestão de chaves *off-chain* combinada com verificação *on-chain*), evidenciando experimentalmente a inviabilidade da verificação Falcon-512 diretamente na SVM devido às restrições do ambiente BPF.
* **Sprint 4:** proposta inicial de um *syscall* nativo no *runtime* da Solana como alternativa às limitações do BPF.
* **Sprint 5:** consolidação da proposta híbrida (Ed25519 com Falcon-512) em formato de artigo, com discussão das implicações de *feature flag*, governança e impacto em custos.

### Módulo 3: Implementação Funcional e Artigo Experimental

Implementação efetiva do *syscall* nativo `sol_falcon512_verify` em um fork do Agave e formalização da arquitetura híbrida com armazenamento da chave pública Falcon-512 em uma PDA derivada da identidade Ed25519 do usuário. Produção do artigo *"Do Ed25519 ao Falcon-512: Viabilidade de uma Transição Pós-Quântica na Solana via Syscall Nativo"*, incluindo benchmark experimental de 20.360 transações no `solana_test_validator`, com medição de tempo de assinatura, tempo de confirmação, tamanho de transação e Compute Units.

### Módulo 4: Versão Final e Submissão Acadêmica

Refinamento do artigo para submissão ao **Congresso Brasileiro de Ciências e Tecnologias Quânticas (CBCTQ 2026)** sob o título *"Viabilidade de uma Transição Pós-Quântica Híbrida na Blockchain Solana via Syscall Nativo"*. A contribuição principal desta etapa foi a introdução do cenário analítico de **Ed25519 sob arquitetura equivalente** (PDA combinado com *syscall* hipotético), permitindo isolar o custo arquitetural do custo criptográfico. O resultado evidencia paridade técnica de **1,09×** entre o Ed25519 hipotético (aproximadamente 8.834 CU) e o Falcon-512 medido (9.614 CU), demonstrando que o *overhead* observado tem origem arquitetural, não algorítmica. Esta etapa também consolidou o TCC e formalizou as limitações (ambiente *single-node*, governança via SIMD).

---

## Estrutura do Repositório

```
.
├── assets/                                            Recursos visuais (logo Inteli)
├── Modulo_1/                                          Artefatos do Módulo 1
│   ├── apresentacao.pdf
│   ├── Arthur_Tsukamoto_public_report.pdf             Relatório público (PT_BR)
│   ├── Arthur_Tsukamoto_public_report_en.pdf          Relatório público (EN)
│   └── public_report.md
├── Modulo_2/                                          Artefatos do Módulo 2
│   ├── Modulo_14_Arthur_Tsukamoto_sprint5.pdf
│   └── README.md
├── Modulo_3/                                          Artefatos do Módulo 3
│   └── Artigo_Modulo_15_Final.pdf
├── Modulo_4/                                          Artefatos do Módulo 4 (TCC final)
│   ├── TCC.pdf                                        Apresentação final
│   └── Viabilidade_de_uma_Transição_Pós_Quântica_Híbrida_na_Blockchain_Solana_via_Syscall_Nativo.pdf
├── LICENSE
└── README.md
```

### Observação

Este repositório contém apenas os resultados consolidados no contexto da Trilha Acadêmica do Inteli. O processo completo de desenvolvimento, incluindo o fork do Agave com a implementação do *syscall* e os scripts de benchmark, está registrado em repositório interno (privado).

---

## Licença

Este repositório é disponibilizado para fins acadêmicos e de pesquisa.
O uso do conteúdo deve citar o autor e o Instituto de Tecnologia e Liderança (Inteli).
