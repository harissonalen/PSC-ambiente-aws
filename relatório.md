# 🌐 Lab AWS — Ambiente de Testes com VPC, EC2 e Acesso SSH

> **Capacita iRede · Nível Intermediário · Unidade 4**

Este repositório documenta a criação manual de um ambiente de testes na **Amazon Web Services (AWS)**, conectando recursos de rede e computação para viabilizar o acesso remoto a uma instância EC2 via SSH.

---

## 📋 Objetivo

Criar um ambiente de testes AWS, compreendendo, na prática, como os principais recursos de rede e computação se conectam para permitir o acesso a uma aplicação na internet, utilizando apenas o **console web da AWS (frontend)**.

---

## 🔧 Recursos Criados

### 🌐 VPC — Virtual Private Cloud
**CIDR:** `10.0.0.0/16`

A VPC é a rede privada virtual isolada onde todos os recursos do ambiente são criados. Ela define um espaço de endereços IP exclusivo, garantindo isolamento lógico entre os recursos da conta AWS. Nenhum tráfego entra ou sai da VPC sem autorização explícita. Funciona como o "datacenter virtual" que contém sub-redes, rotas e instâncias.


![VPC criada](https://github.com/harissonalen/PSC-ambiente-aws/blob/main/prints/criando%20VPC.gif)

---

### 📦 Sub-rede Pública
**CIDR:** `10.0.1.0/24`

A sub-rede é uma subdivisão do espaço de IP da VPC. Por ser pública, está associada a uma tabela de rotas que aponta para o Internet Gateway, permitindo comunicação com a internet. O CIDR `10.0.1.0/24` disponibiliza até 251 endereços utilizáveis. A opção de IP público automático foi ativada para que instâncias lançadas nela recebam automaticamente um endereço IP externo.


![Sub-rede pública](https://raw.githubusercontent.com/harissonalen/PSC-ambiente-aws/main/prints/criando%20sub%20rede.gif)

---

### 🚪 Internet Gateway (IGW)

O Internet Gateway é o componente que conecta a VPC à internet pública. Sem ele, os recursos dentro da VPC ficam completamente isolados, sem capacidade de enviar ou receber tráfego externo. Após criado, ele é anexado à VPC e referenciado pela tabela de rotas como destino do tráfego externo. Age como a "porta de entrada e saída" do ambiente de rede.


![Internet Gateway](https://github.com/harissonalen/PSC-ambiente-aws/blob/main/prints/criando%20IGW%20.gif)

---

### 🗺️ Tabela de Rotas

A tabela de rotas define para onde o tráfego de rede deve ser encaminhado a partir da sub-rede. A rota `0.0.0.0/0 → IGW` instrui que qualquer destino externo seja enviado pelo Internet Gateway. Sem esta rota configurada e associada à sub-rede, mesmo com o IGW presente, o tráfego não chegaria à internet. A associação entre tabela e sub-rede é obrigatória.


![Tabela de rotas](https://github.com/harissonalen/PSC-ambiente-aws/blob/main/prints/criando%20tabela%20de%20rotas%20.gif)

---

### 🔒 Security Group
**Regra:** SSH · Porta `22` · Origem: Meu IP

O Security Group funciona como um firewall virtual no nível da instância EC2. Ele controla quais portas e protocolos são permitidos, tanto para tráfego de entrada (inbound) quanto de saída (outbound). Foi liberada apenas a porta 22 (SSH) para o IP do próprio desenvolvedor, bloqueando qualquer outro acesso não autorizado. Isso segue o princípio do menor privilégio, essencial em ambientes de nuvem.


![Security Group](https://github.com/harissonalen/PSC-ambiente-aws/blob/main/prints/criando%20Grupo%20de%20seguranca%20.gif)

---

### 🖥️ Instância EC2
**AMI:** Amazon Linux 2 · **Tipo:** t2.micro (free tier)

A instância EC2 é o servidor virtual onde a aplicação roda. Foi lançada dentro da sub-rede pública criada, herdando automaticamente um IP público. A chave SSH (`.pem`) gerada no momento do lançamento é o único meio de autenticação na instância. Ela consolida todos os recursos criados, sendo o destino final de acesso do ambiente.


![EC2 em execução](https://github.com/harissonalen/PSC-ambiente-aws/blob/main/prints/criando%20instancia%20EC2.gif)

---

## ✅ Evidência — Acesso SSH

Comando utilizado para acessar a instância:

```bash
ssh -i sua-chave.pem ec2-user@IP_PUBLICO
```


![Acesso SSH funcionando](https://github.com/harissonalen/PSC-ambiente-aws/blob/main/prints/acesso%20a%20EC2.png)

---

## 📌 Conclusão

A atividade demonstrou, na prática, como os componentes de rede da AWS se integram para formar um ambiente funcional e seguro. Ficou claro que cada recurso tem uma responsabilidade bem definida — a ausência de qualquer um deles, como o Internet Gateway ou a rota correta na tabela de rotas, impede o acesso à instância.

O trabalho reforçou conceitos fundamentais de infraestrutura em nuvem como isolamento de rede (VPC), segmentação (sub-redes), roteamento e controle de acesso (Security Group), que são a base de qualquer arquitetura DevOps/Cloud moderna.

---
## Antonio Harisson Alencar Ferreira
*Capacita iRede · Nível Intermediário 
