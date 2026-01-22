# Primeiro Exporter Python para Prometheus

Este é um exporter Python que coleta informações sobre a Estação Espacial Internacional (ISS) de uma API pública e as expõe em formato Prometheus.

## Pré-requisitos

- **Python 3.6 ou superior** instalado no sistema

Para verificar se você tem o Python 3 instalado:
```bash
python3 --version
```

## Instalação e Configuração

### 1. Criar um Virtual Environment (Ambiente Virtual)

Um virtual environment é recomendado para isolar as dependências do projeto:

```bash
# Criar o ambiente virtual
python3 -m venv venv

# Ativar o ambiente virtual
# No Linux/macOS:
source venv/bin/activate

# No Windows (PowerShell):
venv\Scripts\Activate.ps1

# No Windows (CMD):
venv\Scripts\activate.bat
```

Quando ativado corretamente, você verá `(venv)` no início do prompt do terminal.

### 2. Instalar as Dependências

Com o ambiente virtual ativado, instale as dependências do projeto:

```bash
pip install -r requirements.txt
```

As dependências são:
- **requests** - para fazer requisições HTTP à API da ISS
- **prometheus_client** - biblioteca oficial do Prometheus para Python

Para verificar que as dependências foram instaladas corretamente:
```bash
pip list
```

### 3. Executar o Exporter

Com as dependências instaladas e o ambiente virtual ativado:

```bash
python3 exporter.py
```

O exporter iniciará um servidor HTTP que escuta na porta **8899** por padrão.

## Verificando se está Funcionando

Em outro terminal, você pode verificar se o exporter está funcionando:

```bash
curl http://localhost:8899
```

Você verá as métricas no formato Prometheus.

## Integração com Prometheus

Para integrar este exporter com o Prometheus, adicione a seguinte configuração ao arquivo `prometheus.yml`:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'primeiro-exporter'
    static_configs:
      - targets: ['localhost:8899']
```

## Desativar o Ambiente Virtual

Quando terminar de trabalhar com o exporter, desative o ambiente virtual:

```bash
deactivate
```

## Estrutura do Projeto

- `exporter.py` - Código principal do exporter
- `requirements.txt` - Lista de dependências Python
- `Dockerfile` - Arquivo para containerizar o exporter (opcional)

## Solução de Problemas

### Erro: "python3: command not found"
Instale o Python 3:
```bash
# Ubuntu/Debian
sudo apt-get install python3 python3-pip python3-venv

# CentOS/RHEL
sudo yum install python3 python3-pip

# macOS (com Homebrew)
brew install python3
```

### Erro: "ModuleNotFoundError"
Certifique-se de que o ambiente virtual está ativado:
```bash
source venv/bin/activate
```

### A porta 8899 já está em uso
Você pode especificar outra porta no código ou matar o processo que está usando a porta:
```bash
# Encontrar o processo usando a porta 8899
lsof -i :8899

# Ou em sistemas sem lsof
netstat -tlnp | grep 8899
```

## Informações do Exporter

Este exporter coleta as seguintes métricas da ISS:

- Número de astronautas atualmente na Estação Espacial Internacional
- Localização geográfica (latitude e longitude) da ISS
- Dados são atualizados a cada requisição (sem cache)

Os dados vêm da API pública: http://api.open-notify.org/
