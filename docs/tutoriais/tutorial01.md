# Configurando o ambiente básico

Para o desenvolvimento das atividades da disciplina, usaremos as seguintes ferramentas:

- Visual Studio Code (VSCode) como editor de textos com as seguintes extensões:
    - Python
    - Django
- Python
- Framework Django
- Git para versionamento
    - Github como repositório remoto

# Configurando o Git em um computador (primeira vez)

```sh
git config --global user.name "Seu Nome"
git config --global user.email "seuemail@servidor.com"
```

# Para começar um projeto do zero

## Iniciar repositório git
```sh
git init (ou clica no botão do VSCode)
```

## Criar o venv
```sh
python -m venv venv
```

## Ativar o venv (lembrar de usar o Tab)
```sh
./venv/Scripts/Activate.ps1
```

## Para instalar pacotes novos (ou do zero)
```sh
pip install nomedopacote
```

## Para guardar os pacotes instalados (execute sempre que instalar um novo pacote)
```sh
pip freeze > requirements.txt
```

## Fazer commit das alterações
```sh
git add requirements.txt
git commit -m "Mensagem que descreve o que você fez"
```

## Enviar as alterações para GitHub
```sh
git push origin main
```

# Para utilizar um projeto já existente

## Clone o repositório remoto
```sh
git clone https://github.com/nomedodono/nomedorepositorio.git
```

## Criar o venv
```sh
python -m venv venv
```

## Ativar o venv
```sh
./venv/Scripts/Activate.ps1
```

## Instalar os pacotes do requirements.txt
```sh
pip install -r requirements.txt
```
