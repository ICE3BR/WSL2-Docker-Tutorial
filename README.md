# Tutorial: Como Configurar o WSL2 e Docker no Windows

Este tutorial ensinará como instalar e configurar o **WSL2** e o **Docker** no Windows, com foco em desenvolvimento de software.

---

## **1. Instalação do WSL2**

### **Requisitos Pré-Instalação**

- Windows 10 (versão 2004 ou superior) ou Windows 11.
- Virtualização habilitada na BIOS.

#### **Passo 1: Instalar o Windows Terminal**

1. Baixe o **Windows Terminal** pela Microsoft Store:

   - Abra a Microsoft Store e procure por "Windows Terminal".
   - Clique em **Instalar**.

2. Configure o Windows Terminal para abrir como administrador:

   - Abra o **Windows Terminal**.
   - Clique na seta ao lado da aba (+) e selecione **Configurações**.
   - No menu lateral, selecione **Ações** e configure para que o terminal abra sempre como administrador.

#### **Passo 2: Habilitar o WSL**

1. Abra o **Windows Terminal** como administrador.

2. Execute o comando:

   ```bash
   wsl --install
   ```

   - Este comando habilitará o WSL e instalará a distribuição Linux padrão (Ubuntu).

3. Reinicie o computador, se solicitado.

#### **Passo 3: Verificar a Instalação**

Para verificar se o WSL2 está instalado corretamente, execute:

```bash
wsl -l -v
```

- Isso listará as distribuições instaladas e suas versões.
- Se o Ubuntu estiver configurado como **Versão 1**, mude para o WSL2:
  ```bash
  wsl --set-version Ubuntu 2
  ```

---

## **2. Configuração do Docker no WSL2**

O Docker pode ser usado em conjunto com o WSL2 para um ambiente de desenvolvimento eficiente.

### **Opção 1: Usar Docker Desktop**

#### **Passo 1: Instalar o Docker Desktop**

1. Baixe o instalador do Docker Desktop em [docker.com](https://www.docker.com/products/docker-desktop/).
2. Durante a instalação, certifique-se de que a opção **Enable WSL 2 Features** esteja habilitada.
3. Conclua a instalação e reinicie o sistema, se solicitado.

#### **Passo 2: Configurar o Docker Desktop para o WSL2**

1. Abra o Docker Desktop.
2. Navegue até **Settings > Resources > WSL Integration**.
3. Habilite a distribuição Linux desejada (ex.: Ubuntu) e clique em **Apply & Restart**.

#### **Passo 3: Testar o Docker**

No terminal do WSL2, execute:

```bash
docker --version
docker run hello-world
```

- Se o contêiner "hello-world" rodar com sucesso, o Docker está configurado corretamente.

---

### **Opção 2: Instalar o Docker Engine Diretamente no WSL2**

Essa opção é recomendada para quem deseja um ambiente mais leve, sem o Docker Desktop.

#### **Passo 1: Instalar Dependências**

No terminal do WSL2, execute:

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

#### **Passo 4: Permitir Uso do Docker sem ****`sudo`**

Adicione seu usuário ao grupo Docker:

```bash
sudo usermod -aG docker $USER
```

Reinicie o WSL2:

```bash
wsl --shutdown
```

Depois, abra novamente o WSL e teste:

```bash
docker run hello-world
```

---

## **3. Configurações Avançadas do WSL2 (Opcional)**

### **Arquivo ****`.wslconfig`**

Você pode personalizar o uso de recursos do WSL2 (CPU, memória, etc.) editando o arquivo `.wslconfig`:

1. Crie o arquivo em `C:\Users\<seu_usuario>\`.
2. Adicione o seguinte conteúdo:
   ```conf
   [wsl2]
   memory=4GB
   processors=2
   swap=2GB
   localhostForwarding=true
   ```
3. Reinicie o WSL:
   ```bash
   wsl --shutdown
   ```

---

Com essas etapas, você terá o **WSL2** e o **Docker** configurados corretamente para um ambiente de desenvolvimento. Em próximos passos, podemos adicionar guias para ferramentas como **pip**, **pipx**, **git**, e o uso do **VSCode** para um setup ainda mais completo.

---

### **Licença**

- Licença: MIT
- Autor: ICE3BR
- Ano: 2025
- Repositório no GitHub: [https://github.com/ICE3BR/WSL2-Docker-Tutorial](https://github.com/ICE3BR/WSL2-Docker-Tutorial)

