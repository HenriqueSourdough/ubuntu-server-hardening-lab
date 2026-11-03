# 🛡️ Network Security Assessment & Server Hardening Lab

Este repositório documenta a execução prática de uma auditoria de segurança de rede (*Security Assessment*), captura forense de pacotes e implementação de contramedidas de blindagem de servidores (*Server Hardening*) em ambiente GNU/Linux.

O projeto demonstra o ciclo completo de análise da superfície de ataque, coleta de evidências de tráfego em tempo real e aplicação de políticas restritivas de firewall baseadas no princípio *Default-Deny*.

## 📐 Arquitetura do Laboratório

A topologia foi implantada em ambiente virtualizado, simulando um cenário real de rede local segmentada.

- **Servidor Alvo (Hardening):** Ubuntu Server (IP: `192.168.3.150`)
- **Estação de Auditoria (Atacante):** Windows 10 (Executando Nmap)
- **Modo de Rede:** Placa em modo *Bridge* (Adaptador em Ponte)
- **Serviço de Gerenciamento:** OpenSSH (porta `22/tcp`)
- **Ferramentas Utilizadas:** Nmap, Tcpdump e UFW (*Uncomplicated Firewall*)

## 🚀 Fases de Execução

### 1. Reconhecimento Inicial e Captura Forense de Tráfego

Antes de aplicar qualquer regra de defesa, iniciou-se a escuta passiva na interface de rede do servidor para registrar o tráfego de varredura.

**Iniciação da captura com o Tcpdump:**

```
sudo tcpdump -i enp0s3 -w evidencia_scan.pcapng
```

A partir da estação Windows, conduziu-se um escaneamento completo, mapeando todas as portas TCP (`-p-`), identificando versões de serviços (`-sV`) e executando scripts de enumeração (`-sC`).

**Execução da varredura com o Nmap:**

```
nmap -sV -sC -p- 192.168.3.150
```

### 2. Implementação do Server Hardening (UFW)

Para mitigar vetores de ataque e fechar portas desnecessárias, configurou-se o firewall nativo do Ubuntu Server sob uma diretriz estrita de segurança.

#### 2.1. Definição das políticas padrão (*Default-Deny*)

```
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

#### 2.2. Abertura seletiva do canal de administração (SSH)

```
sudo ufw allow 22/tcp
```

#### 2.3. Ativação do Firewall e Auditoria de Estado

```
sudo ufw enable
sudo ufw status verbose
```

## 📊 Validação e Resultados

Após a aplicação das diretrizes de blindagem, o perfil de exposição do servidor reduziu-se drasticamente:

- **Portas filtradas / ocultadas:** 65.534 portas TCP sem resposta.
- **Superfície de ataque ativa:** Exclusivamente a porta `22/tcp` (OpenSSH), mantendo o gerenciamento remoto operacional.

## 📂 Estrutura do Repositório e Evidências

```
ubuntu-server-hardening-lab/
│
├── assets/
│   └── screenshots/
│       ├── nmap_scan.png
│       ├── ufw_config.png
│       └── ufw_status.png
│
├── captures/
│   └── evidencia_scan.pcapng   # Gerido via Git LFS (~905 MB)
│
└── README.md
```

### Descrição dos Recursos

- **`assets/screenshots/`:** Contém os registros visuais do terminal e das ferramentas de escaneamento.
- **`captures/evidencia_scan.pcapng`:** Arquivo bruto contendo o registro de mais de 130 mil pacotes capturados pelo Tcpdump, permitindo inspeção detalhada no Wireshark.

## 🎯 Competências Demonstradas

- Auditoria e avaliação de segurança de redes locais.
- Enumeração de serviços e detecção de portas com o Nmap.
- Captura forense de pacotes com o Tcpdump.
- Administração de firewalls baseados em iptables/UFW em sistemas Linux.
- Implementação prática do princípio do menor privilégio (*Least Privilege*) e *Server Hardening*.
