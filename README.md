# Tutorial: Configuração do WSL2 e Docker no Windows

Este repositório contém um guia detalhado e organizado para configurar o WSL2 e Docker no Windows, com foco em um ambiente de desenvolvimento eficiente e escalável.

---
<!-- https://github.com/codeedu/wsl2-docker-quickstart -->
## Sumário

  - [Introdução](#introdução)
  - [Requisitos Pré-Instalação](#requisitos-pré-instalação)
  - [Licença e Informações](#licença-e-informações)

<details open>
<summary>
    <strong>WSL</strong>
</summary>

  - [Instalação do WSL2](#instalação-do-wsl2)
    - [Instalar o Windows Terminal](#passo-1-instalar-o-windows-terminal)
    - [Habilitar o WSL](#passo-2-habilitar-o-wsl)
    - [Verificar a Instalação](#passo-3-verificar-a-instalação)
</details>

<details open>
<summary>
    <strong>Docker</strong>
</summary>

- [Configuração do Docker no WSL2](#configuração-do-docker-no-wsl2)
    - [Opção 1: Docker Desktop](#opção-1-docker-desktop)
    - [Opção 2: Docker Engine no WSL2](#opção-2-docker-engine-no-wsl2)
- [Configurações Avançadas do WSL2](#configurações-avançadas-do-wsl2)


</details>

<details open>
<summary>
    <strong>Ambiente Virtual Python</strong>
</summary>

- [Instalando o pipx](#instalando-o-pipx)

</details>

<details open>
<summary>
    <strong>VSCode</strong>
</summary>

- [Baixando e Configurando o Python](#baixando-e-configurando-o-python)

- [Abrindo o VSCode com o WSL2](#abrindo-o-vscode-com-o-wsl2)

</details>

<!-- 
<details open>
<summary>
    <strong>XXX</strong>
</summary>

- [XXX](XXX)
  - [XXX](XXX)

</details>
-->
---

## **Introdução**

O **Windows Subsystem for Linux 2 (WSL2)** e o **Docker** são ferramentas fundamentais para desenvolvedores que desejam trabalhar com Linux dentro do Windows. Este guia fornece passos claros e organizados para configurar ambos de forma eficiente.

---

## **Requisitos Pré-Instalação**

- Windows 10 (versão 2004 ou superior) ou Windows 11.
- Virtualização habilitada na BIOS.
- Acesso à internet para baixar dependências.

---

## **Instalação do WSL2**

### **Passo 1: Instalar o Windows Terminal**
1. Baixe o **Windows Terminal** pela Microsoft Store:
   - Abra a Microsoft Store e procure por "Windows Terminal".
   - Clique em **Instalar**.
2. Configure o Windows Terminal para abrir como administrador:
   - Abra o **Windows Terminal**.
   - Clique na seta ao lado da aba (+) e selecione **Configurações**.
   - No menu lateral, configure para que o terminal abra sempre como administrador.

### **Passo 2: Habilitar o WSL**
1. Abra o **Windows Terminal** como administrador.
2. Execute o comando:
   ```bash
   wsl --install
   ```
   - Este comando habilitará o WSL e instalará a distribuição Linux padrão (Ubuntu).
3. Reinicie o computador, se solicitado.

### **Passo 3: Verificar a Instalação**
Execute o comando:
```bash
wsl -l -v
```
- Isso listará as distribuições instaladas e suas versões.
- Se necessário, mude para o WSL2:
  ```bash
  wsl --set-version Ubuntu 2
  ```

---

## **Configuração do Docker no WSL2**

### **Opção 1: Docker Desktop**

1. Baixe o Docker Desktop em [docker.com](https://www.docker.com/products/docker-desktop/).
2. Durante a instalação, habilite a opção **Enable WSL 2 Features**.
3. Abra o Docker Desktop, vá até **Settings > Resources > WSL Integration**, e habilite a integração com o Ubuntu.
4. Teste o Docker no terminal do WSL:
   ```bash
   docker --version
   docker run hello-world
   ```

### **Opção 2: Docker Engine no WSL2**

#### **Passo 1: Instalar Dependências**
```bash
sudo apt update
sudo apt install ca-certificates curl gnupg lsb-release
```

#### **Passo 2: Adicionar o Repositório do Docker**
```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

#### **Passo 3: Instalar o Docker**
```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

#### **Passo 4: Permitir Uso sem `sudo`**
```bash
sudo usermod -aG docker $USER
wsl --shutdown
```
Depois, reabra o WSL e teste:
```bash
docker run hello-world
```

---

## **Configurações Avançadas do WSL2**

### **Ajustes no Arquivo `.wslconfig`**
1. Crie o arquivo `C:\Users\<seu_usuario>\.wslconfig` com o seguinte conteúdo:
   ```conf
   [wsl2]
   memory=4GB
   processors=2
   swap=2GB
   localhostForwarding=true
   ```
2. Reinicie o WSL com:
   ```bash
   wsl --shutdown
   ```

---

## **Baixando e Configurando o Python**

### **Passo 1: Atualizar o Sistema**
1. Abra o terminal no WSL2.
2. Execute o comando para garantir que todos os pacotes estão atualizados:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

### **Passo 2: Instalar o Python**
1. Instale o Python 3 com o comando:
   ```bash
   sudo apt install python3 python3-pip -y
   ```
2. Verifique se o Python foi instalado corretamente:
   ```bash
   python3 --version
   pip3 --version
   ```
   - Isso exibirá a versão do Python e do pip instalados.

### **Passo 3: Configuração Opcional**
- Caso deseje configurar o Python como comando padrão, crie um alias no terminal:
   ```bash
   echo "alias python=python3" >> ~/.bashrc
   echo "alias pip=pip3" >> ~/.bashrc
   source ~/.bashrc
   ```

---

## **Instalando o pipx**

### **Passo 1: Instalar o pipx**
1. Certifique-se de que o Python e o pip estão instalados corretamente (veja a seção anterior).
2. Instale o pipx usando o pip:
   ```bash
   pip install --user pipx
   ```
3. Adicione o diretório do pipx ao seu PATH:
   ```bash
   python3 -m pipx ensurepath
   ```
4. Reinicie o terminal ou execute:
   ```bash
   source ~/.bashrc
   ```
5. Verifique a instalação do pipx:
   ```bash
   pipx --version
   ```
   - Isso exibirá a versão do pipx instalada.

### **Passo 2: Usar o pipx para Instalar Pacotes**
O pipx permite instalar e executar pacotes Python isolados. Por exemplo, para instalar o **[uv](https://docs.astral.sh/uv/)**:
   ```bash
   pipx install uv
   ```
   Após a instalação, você pode executar o pacote diretamente:
   ```bash
   uv --help
   ```

---

## **Abrindo o VSCode com o WSL2**

### **Passo 1: Abrir o VSCode Usando `code .`**
1. No terminal do WSL2, navegue até o diretório do projeto desejado.
   ```bash
   cd /caminho/para/seu/projeto
   ```
2. Execute o comando para abrir o Visual Studio Code:
   ```bash
   code .
   ```
   - Isso abrirá o VSCode conectado diretamente ao ambiente do WSL2.

### **Passo 2: Instalar Extensões Necessárias**
1. No VSCode, acesse a aba de extensões (ícone de quadrados no menu lateral ou `Ctrl+Shift+X`).
2. Procure e instale as seguintes extensões:
   - **[Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)** (Pacote que permite acessar o WSL2 diretamente pelo VSCode).
   - Outras extensões relacionadas ao seu projeto (ex.: Docker, Python, Node.js).


### **Passo 3: Conexão Remota pelo VSCode no Windows**
- Após instalar a extensão **Remote Development**, você pode se conectar ao WSL2 a qualquer momento diretamente do VSCode.
- Clique na aba inferior esquerda **(><)** e selecione **Connect to WSL**.

---

## **Licença e Informações**

- **Licença**: MIT
- **Autor**: ICE3BR
- **Ano**: 2025
- **Repositório no GitHub**: [https://github.com/ICE3BR/WSL2-Docker-Tutorial](https://github.com/ICE3BR/WSL2-Docker-Tutorial)

