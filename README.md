# 🛡️ Network Security Assessment & Server Hardening Lab

Este repositório documenta a execução prática de uma auditoria de segurança de rede (*Security Assessment*), captura forense de pacotes e implementação de contramedidas de blindagem de servidor (*Server Hardening*) em ambiente Linux.

O projeto demonstra o ciclo completo de análise de vulnerabilidades da superfície de ataque, coleta de evidências de tráfego e aplicação de políticas restritivas de firewall.

---

## 📐 Arquitetura do Laboratório

A topologia foi implantada em ambiente virtualizado, com comunicação direta em camada de rede local.

- **Servidor Alvo (Hardening):** Ubuntu Server
- **Estação de Auditoria (Atacante):** Windows 10 (Nmap e Wireshark)
- **Modo de Rede:** Placa em modo Bridged (Bridged Adapter)
- **Serviço de Gerenciamento:** OpenSSH (porta 22/tcp)
- **Ferramentas Utilizadas:** Nmap, Tcpdump e UFW (Uncomplicated Firewall)

---

## 🚀 Etapas da Execução

### 1. Levantamento Inicial e Captura Forense

Antes de aplicar qualquer regra de defesa, foi iniciada a escuta da interface de rede no servidor Linux para geração de evidências forenses.

**Captura de tráfego com Tcpdump:**

```
sudo tcpdump -i enp0s3 -w evidencia_scan.pcapng
```

A partir da máquina Windows 10, executou-se uma varredura completa, cobrindo o espectro total de portas TCP (`-p-`), identificação de versões dos serviços (`-sV`) e execução de scripts de enumeração padrão (`-sC`).

**Varredura com Nmap:**

```
nmap -sV -sC -p- 192.168.3.150
```

### 2. Implementação do Server Hardening (UFW)

Para mitigar a exposição de serviços e vetores de ataque não autorizados no Ubuntu Server, aplicou-se uma política restritiva de *Default-Deny* utilizando o firewall nativo UFW.

**2.1. Definição das políticas padrão:**

```
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

**2.2. Liberação estrita do serviço essencial (SSH):**

```
sudo ufw allow 22/tcp
```

**2.3. Ativação e verificação do firewall:**

```
sudo ufw enable
sudo ufw status verbose
```

---

## 📊 Resultados e Validação

Após a aplicação do hardening, uma nova varredura Nmap foi realizada a partir da máquina auditora.

O resultado demonstrou a eficácia das regras de firewall na redução da superfície de ataque:

- **Portas filtradas pelo firewall:** 65.534 portas TCP sem resposta que permitisse confirmar sua disponibilidade.
- **Superfície exposta permitida:** Apenas a porta 22/tcp (OpenSSH) permaneceu acessível para administração remota.

**Resultado da varredura após o hardening:**

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 10.2p1 Ubuntu
```

---

## 📂 Estrutura de Arquivos e Evidências

```
ubuntu-server-hardening-lab/
│
├── README.md
│
├── assets/
│   ├── scan-inicial-nmap.png
│   └── scan-pos-hardening.png
│
└── captures/
    └── evidencia_scan.pcapng
```

### Descrição das Evidências

- `assets/scan-inicial-nmap.png`: Captura de tela da varredura inicial, demonstrando a superfície de ataque antes da aplicação das regras de firewall.
- `assets/scan-pos-hardening.png`: Captura de tela da varredura posterior, demonstrando a redução da exposição dos serviços após o hardening.
- `captures/evidencia_scan.pcapng`: Arquivo contendo o registro bruto do tráfego de rede capturado durante as varreduras ativas, permitindo análise posterior no Wireshark.

---

## 🎯 Objetivo do Projeto

Demonstrar, na prática, conhecimentos fundamentais de:

- Auditoria e avaliação de segurança de redes.
- Identificação e enumeração de serviços com Nmap.
- Captura e análise forense de pacotes com Tcpdump e Wireshark.
- Configuração e administração de firewall Linux com UFW.
- Aplicação de políticas de segurança baseadas no princípio *Default-Deny*.
- Redução da superfície de ataque por meio de Server Hardening.
