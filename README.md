# Trilha Acadêmica – Inteli

<p align="center"> 
  <img src="assets/img/inteli_logo.png" width="180"> 
</p> 

<h1 align="center"> 
  Proposta de Transição para Criptografia Pós-Quântica na Solana 
</h1> 

<p align="center"> 
   <strong>Trilha Acadêmica – Instituto de Tecnologia e Liderança (Inteli)</strong><br> 
   Trabalho de Conclusão de Curso
  </p>

## 👥 Autor

**Autor:** Arthur Tsukamoto
**Curso:** Ciência da Computação
**Orientadora:** Profa. Dra. Ana Cristina Santos
**Área:** Criptografia Pós-Quântica, Blockchain, Sistemas Distribuídos

---

## 🎓 Sobre o Projeto

Este projeto foi desenvolvido no âmbito da Trilha Acadêmica do Inteli como parte do Trabalho de Conclusão de Curso.

O estudo investiga a viabilidade técnica da integração de criptografia pós-quântica (PQC) na blockchain Solana, analisando limitações arquiteturais do runtime, possibilidades de modelos híbridos (on-chain / off-chain) e propostas de extensão via syscall nativa.

---

## 🔎 Problema de Pesquisa

Diante da ameaça quântica aos esquemas criptográficos clássicos (como Ed25519),

é viável integrar algoritmos pós-quânticos de assinatura digital à Solana de forma nativa e eficiente, preservando segurança e desempenho?

---

## 🎯 Objetivo

Avaliar tecnicamente a transição para criptografia pós-quântica na Solana por meio de:
- Benchmarks de algoritmos PQC em Rust
- Implementação de modelo híbrido (Ed25519 + PQC)
- Tentativa de verificação on-chain
- Proposta de extensão do runtime via syscall nativa

---

## 🧪 Abordagem Metodológica

O projeto foi desenvolvido em três estudos principais:

### 📊 Estudo 1 — Benchmark PQC Off-Chain
Avaliação de desempenho, tamanhos de chave e assinatura.

### 🔗 Estudo 2 — Modelo Híbrido On-Chain / Off-Chain
Integração experimental de verificação PQC com arquitetura híbrida.

### ⚙️ Estudo 3 — Extensão do Runtime (Syscall Nativa)
Modificação experimental do fork do validator e implementação de syscall PQC.

---

## 📊 Resultados Principais

- Avaliação comparativa de desempenho entre algoritmos PQC em Rust.
- Implementação funcional de modelo híbrido (Ed25519 + PQC).
- Tentativa experimental de extensão do runtime via syscall nativa.
- Identificação de barreiras arquiteturais para adoção nativa.

---

### 📄 Documento Científico

O relatório público consolidado está disponível em:

- 📎 Arthur_Tsukamoto_public_report.pdf
- 📎 Arthur_Tsukamoto_public_report_en.pdf
  
---

## 🛠 Tecnologias Utilizadas

- Rust
- Solana Validator (fork)
- BPF Runtime
- Algoritmos PQC (Falcon-512, ML-DSA, etc.)
- Cargo / Toolchain Rust

---

## 📂 Estrutura do Repositório

/Modulo_1 – Documentação e artefatos do Módulo 1  
/Modulo_2 – Documentação e artefatos do Módulo 2  
/Modulo_3 – Documentação e artefatos do Módulo 3  

Arthur_Tsukamoto_public_report.pdf – Relatório técnico consolidado  
Arthur_Tsukamoto_public_report_en.pdf – Versão em inglês  
apresentacao.pdf – Apresentação final do projeto  

---

### 📌 Observação

Este repositório contém apenas os resultados consolidados do módulo no contexto da Trilha Acadêmica do Inteli.  
O processo completo de desenvolvimento está registrado em repositório interno (privado).


### 📜 Licença

Este repositório é disponibilizado para fins acadêmicos e de pesquisa.
O uso do conteúdo deve citar o autor e o Instituto de Tecnologia e Liderança (Inteli).
