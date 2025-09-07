# 🚀 Configuração Avançada para Programação Competitiva

<div align="center">

![Programação Competitiva](https://img.shields.io/badge/Programação-Competitiva-blueviolet?style=for-the-badge)
![Codeforces](https://img.shields.io/badge/Codeforces-1F8ACB?style=for-the-badge&logo=codeforces&logoColor=white)
![AtCoder](https://img.shields.io/badge/AtCoder-231815?style=for-the-badge)
![ICPC](https://img.shields.io/badge/ICPC-003399?style=for-the-badge)

</div>

<p align="center">Minha configuração pessoal para competições de programação (Codeforces, AtCoder, ICPC, etc.), inspirada no setup do competidor de elite Neal Wu.</p>

---

## 📋 Índice

- [Requisitos do Sistema](#-requisitos-do-sistema)
- [Instalação](#-instalação)
- [Scripts e Ferramentas](#-scripts-e-ferramentas)
- [Arquivos de Configuração](#-arquivos-de-configuração)
- [Guia de Uso](#-guia-de-uso)
- [Configuração do IDE](#-configuração-do-ide)
- [Dicas e Truques](#-dicas-e-truques)

---

## 💻 Requisitos do Sistema

| Componente | Recomendação |
|-----------|---------------|
| **Sistema Operacional** | Ubuntu 22.04 LTS |
| **Editor/IDE** | Sublime Text 4 (Versão 4180) |
| **Compilador** | g++ (GCC) |
| **Debugger** | gdb (GDB) |

---

## 🔧 Instalação

### 1. Clone o Repositório

```bash
git clone https://github.com/glaucoacassio2020/MY_CP.git
```

### 2. Crie a Estrutura de Diretórios

Crie um diretório na sua pasta pessoal do Ubuntu chamado `Sublime` para armazenar todos os arquivos do programa:

```bash
mkdir -p ~/Sublime
```

Este diretório conterá os arquivos principais:
- brute.cpp
- download_prob.py
- download_problem.py
- gen.cpp
- gen_cf.py
- gen_tree.cpp
- gen_tree2.cpp
- s.sh
- template.cc

Dentro do diretório `Sublime`, organize suas soluções em subdiretórios por plataforma e competição. Cada pasta de problema terá seus próprios arquivos de código-fonte, casos de teste e scripts:

```
Sublime/
├── Codeforces/
│   ├── Codeforces Round 991 (Div. 3)/
│   │   ├── A-1.in        # Arquivo de entrada para o caso de teste 1 do problema A
│   │   ├── A-1.out       # Saída esperada para o caso de teste 1 do problema A
│   │   ├── A-2.in        # Arquivo de entrada para o caso de teste 2 do problema A
│   │   ├── A-2.out       # Saída esperada para o caso de teste 2 do problema A
│   │   ├── A.cc          # Código-fonte da solução para o problema A
│   │   ├── B-1.in        # Arquivo de entrada para o caso de teste 1 do problema B
│   │   ├── B-1.out       # Saída esperada para o caso de teste 1 do problema B
│   │   ├── B.cc          # Código-fonte da solução para o problema B
│   │   ├── gen_cf.py     # Script para baixar problemas do Codeforces
│   │   └── s.sh          # Script de teste
│   └── Educational Codeforces Round 175/
│       └── ...           # Mesma estrutura para outra competição
├── AtCoder/
│   ├── Beginner Contest 300/
│   │   ├── A-1.in
│   │   ├── A-1.out
│   │   ├── A.cc
│   │   └── ...
│   └── Regular Contest 150/
│       └── ...
├── CodeChef/
│   ├── Long Challenge October 2025/
│   │   ├── PROBLEM1-1.in
│   │   ├── PROBLEM1-1.out
│   │   ├── PROBLEM1.cc
│   │   └── ...
│   └── Cook-Off 2025/
│       └── ...
├── Google Code Jam/
│   └── 2025/
│       ├── Qualification/
│       │   ├── A-1.in
│       │   ├── A-1.out
│       │   ├── A.cc
│       │   └── ...
│       └── Round 1/
│           └── ...
├── ICPC/
│   └── Regional 2025/
│       ├── A-1.in
│       ├── A-1.out
│       ├── A.cc
│       └── ...
└── Others/
    └── ...
```

> [!TIP]
> Esta estrutura organizada facilita a execução dos comandos `runsamples` e `dbrun`. Por exemplo, para testar a solução do problema A do Codeforces Round 991, navegue até a pasta da competição e execute `runsamples A`, que automaticamente encontrará os arquivos A-1.in, A-1.out etc.

### 3. Instale o Python e Dependências

```bash
# Atualizar listas de pacotes
sudo apt update

# Instalar Python3 se ainda não estiver instalado
sudo apt install python3

# Instalar pip3
sudo apt install python3-pip

# Instalar módulos Python necessários
pip3 install docopt

# Método alternativo para instalar docopt
sudo apt install python3-docopt
```

---

## 📜 Scripts e Ferramentas

### Configurar o Comando `runsamples`

1. Crie o diretório bin se ele não existir:

```bash
mkdir -p ~/bin
```

2. Crie o script `runsamples`:

```bash
gedit ~/bin/runsamples
```

3. Adicione o seguinte conteúdo:

```bash
#!/bin/bash
# Cores para formatação
GREEN='\033[0;32m'
RED='\033[0;31m'
BLUE='\033[0;34m'
YELLOW='\033[1;33m'
WHITE='\033[1;37m'
PURPLE='\033[0;35m'  # Cor roxa para avisos específicos
NC='\033[0m' # Sem cor
# Funções para medir uso de memória e tempo
measure_memory() {
    /usr/bin/time -f "%M" $@ 2>&1 > /dev/null | tail -n 1
}
# Verifica se foi fornecido um argumento
if [ $# -eq 0 ]; then
    echo "Usage: runsamples <letter_of_the_problem>"
    exit 1
fi
# Pega apenas a primeira letra do argumento
LETRA="${1:0:1}"
ARQUIVO="${LETRA}.cc"
# Verifica se o arquivo .cc existe
if [ ! -f "$ARQUIVO" ]; then
    echo -e "${RED}Error: File $ARQUIVO not found.${NC}"
    exit 1
fi
# Determina a versão do C++ mais recente disponível
CXX_VERSION="c++17"
if g++ -std=c++20 -dM -E -x c++ /dev/null > /dev/null 2>&1; then
    CXX_VERSION="c++20"
elif g++ -std=c++2a -dM -E -x c++ /dev/null > /dev/null 2>&1; then
    CXX_VERSION="c++2a"
fi
# Compila o arquivo COM A DIRETIVA -DDEBUG
echo -e "${BLUE}[DEBUG MODE]${NC} Compiling ${YELLOW}$ARQUIVO${NC} with ${YELLOW}$CXX_VERSION${NC}."
# Captura avisos de compilação em uma variável
COMPILE_OUTPUT=$(g++ -std=$CXX_VERSION -O2 -Wall -DDEBUG -o "$LETRA" "$ARQUIVO" 2>&1)
COMPILE_STATUS=$?

if [ $COMPILE_STATUS -ne 0 ]; then
    echo -e "${RED}Compilation failed!${NC}"
    echo "$COMPILE_OUTPUT"
    exit 1
fi

# Processa os avisos para colorir, mas não exibe ainda
if [ ! -z "$COMPILE_OUTPUT" ]; then
    FORMATTED_OUTPUT=""
    while IFS= read -r line; do
        # Substituir partes específicas para destacar em roxo
        if [[ "$line" == *"warning:"* ]]; then
            line="${line/warning:/${PURPLE}warning:${NC}}"
        fi
        if [[ "$line" == *"-Wsign-compare"* ]]; then
            line="${line/-Wsign-compare/${PURPLE}-Wsign-compare${NC}}"
        fi
        if [[ "$line" == *"<="* || "$line" == *">="* || "$line" == *"=="* ]]; then
            # Encontrar a parte da expressão que contém o operador
            if [[ "$line" =~ ([a-zA-Z0-9_+. ()]+[<>=]{1,2}[a-zA-Z0-9_+. ()]+) ]]; then
                expr="${BASH_REMATCH[1]}"
                line="${line/$expr/${PURPLE}$expr${NC}}"
            fi
        fi
        if [[ "$line" == *"~"* ]]; then
            line="${PURPLE}$line${NC}"
        fi
        FORMATTED_OUTPUT+="$line\n"
    done <<< "$COMPILE_OUTPUT"
fi

# Encontra todos os casos de teste usando find para maior confiabilidade
TEST_FILES=($(find . -maxdepth 1 -name "${LETRA}-*.in" | sort))
TOTAL_TESTS=${#TEST_FILES[@]}
PASSED_TESTS=0
if [ $TOTAL_TESTS -eq 0 ]; then
    echo -e "${YELLOW}No test cases found in format ${LETRA}-*.in${NC}"
    
    if [ -f "s.sh" ]; then
        echo -e "${BLUE}Running s.sh with ./$LETRA...${NC}"
        chmod +x s.sh
        ./s.sh ./"$LETRA"
    fi
    
    # Mesmo sem testes, mostrar o resumo
    echo -e "${GREEN}$PASSED_TESTS / $TOTAL_TESTS tests passed${NC}\n"
    exit 0
fi
# Executar cada caso de teste
for IN_FILE in "${TEST_FILES[@]}"; do
    # Extrair apenas o nome do arquivo sem o caminho
    FILENAME=$(basename "$IN_FILE")
    NUM=$(echo "$FILENAME" | sed -E "s/${LETRA}-([0-9]+)\.in/\1/")
    OUT_FILE="$(dirname "$IN_FILE")/${LETRA}-${NUM}.out"
    
    echo -e "${WHITE}Running ${LETRA}-${NUM}.in:${NC}"
    
    # Mostrar avisos de compilação APÓS o Running A-X.in
    if [ ! -z "$FORMATTED_OUTPUT" ]; then
        echo -e "$FORMATTED_OUTPUT"
        # Só exibimos na primeira vez
        FORMATTED_OUTPUT=""
    fi
    
    # Cria arquivos temporários para diferentes saídas
    TEMP_OUT=$(mktemp)
    TEMP_ERR=$(mktemp)
    
    # Medir tempo e uso de memória
    START_TIME=$(date +%s.%N)
    
    # Executa o programa e separa stdout e stderr
    { ./"$LETRA" < "$IN_FILE" > "$TEMP_OUT"; } 2> "$TEMP_ERR"
    
    END_TIME=$(date +%s.%N)
    
    # Exibe as mensagens de debug (stderr)
    if [ -s "$TEMP_ERR" ]; then
        cat "$TEMP_ERR"
    fi
    
    # Calcula o tempo de execução
    EXEC_TIME=$(echo "$END_TIME - $START_TIME" | bc)
    # Formata o tempo para apenas 3 casas decimais com LC_NUMERIC=C para garantir o ponto como separador
    EXEC_TIME=$(LC_NUMERIC=C printf "%.3f" $EXEC_TIME)
    MEMORY_KB=$(measure_memory ./"$LETRA" < "$IN_FILE")
    
    # Depois exibe as informações de memória e tempo
    echo -e "${WHITE}Memory: $MEMORY_KB Kb${NC}"
    echo -e "${WHITE}Time: ${EXEC_TIME}s${NC}"
    echo -e "${WHITE}---------------------------------${NC}"
    
    # Por fim, exibe a saída e o esperado
    echo -e "${WHITE}Output:${NC}"
    cat "$TEMP_OUT"
    echo -e "${WHITE}---------------------------------${NC}"
    
    # Quando for verificar a diferença entre arquivos
    if [ -f "$OUT_FILE" ]; then
        echo -e "${WHITE}Expected:${NC}"
        cat "$OUT_FILE"
        echo -e "${WHITE}---------------------------------${NC}"
        
        if diff -w "$TEMP_OUT" "$OUT_FILE" > /dev/null; then
            echo -e "${GREEN}Passed!${NC}"
            ((PASSED_TESTS++))
        else
            # Gerar um nome temporário descritivo com a letra do problema
            TEMP_NAME="run_samples_output-${LETRA}-$(date +%s%N | cut -b1-8)"
            
            # Gera arrays com as linhas de cada arquivo
            IFS=$'\n' read -d '' -ra ACTUAL_LINES < "$TEMP_OUT"
            IFS=$'\n' read -d '' -ra EXPECTED_LINES < "$OUT_FILE"
            
            # Conta o número total de linhas
            ACTUAL_COUNT=${#ACTUAL_LINES[@]}
            EXPECTED_COUNT=${#EXPECTED_LINES[@]}
            
            # Determina o número máximo de linhas a verificar
            MAX_LINES=$(( ACTUAL_COUNT > EXPECTED_COUNT ? ACTUAL_COUNT : EXPECTED_COUNT ))
            
            # Compara todas as linhas sem limite
            for (( i=0; i<MAX_LINES; i++ )); do
                ACTUAL="${ACTUAL_LINES[$i]:-}"
                EXPECTED="${EXPECTED_LINES[$i]:-}"
                
                if [ "$ACTUAL" != "$EXPECTED" ]; then
                    LINE_NUM=$((i+1))
                    echo -e "${RED}Mismatch at line $LINE_NUM: '$ACTUAL' ($TEMP_NAME) vs. '$EXPECTED' (${LETRA}-${NUM}.out)${NC}"
                fi
            done
            
            echo -e "${RED}Failed!${NC}"
        fi
    else
        echo -e "${YELLOW}Output file ${OUT_FILE} not found. Unable to verify result.${NC}"
    fi
    
    echo ""
    rm "$TEMP_OUT" "$TEMP_ERR"
done
# Resumo dos testes
if [ $PASSED_TESTS -eq $TOTAL_TESTS ]; then
    echo -e "${GREEN}$PASSED_TESTS / $TOTAL_TESTS tests passed${NC}\n"
else
    echo -e "${RED}$PASSED_TESTS / $TOTAL_TESTS tests passed${NC}\n"
fi
```

### Configurar o Comando `dbrun`

1. Crie o script:

```bash
gedit ~/bin/dbrun
```

2. Adicione o seguinte conteúdo:

```bash
#!/bin/bash
# Cores para formatação
GREEN='\033[0;32m'
RED='\033[0;31m'
BLUE='\033[0;34m'
YELLOW='\033[1;33m'
WHITE='\033[1;37m'
PURPLE='\033[0;35m'
NC='\033[0m' # Sem cor
# Verifica se foi fornecido um argumento
if [ $# -eq 0 ]; then
    echo "Usage: dbrun <letter_of_the_problem>"
    exit 1
fi
# Pega apenas a primeira letra do argumento
LETRA="${1:0:1}"
ARQUIVO="${LETRA}.cc"
# Verifica se o arquivo .cc existe
if [ ! -f "$ARQUIVO" ]; then
    echo -e "${RED}Error: File $ARQUIVO not found.${NC}"
    exit 1
fi
# Determina a versão do C++ mais recente disponível
CXX_VERSION="c++17"
if g++ -std=c++20 -dM -E -x c++ /dev/null > /dev/null 2>&1; then
    CXX_VERSION="c++20"
elif g++ -std=c++2a -dM -E -x c++ /dev/null > /dev/null 2>&1; then
    CXX_VERSION="c++2a"
fi
# Compila o arquivo com a diretiva -DDEBUG
echo -e "${BLUE}[DEBUG MODE]${NC} Compiling ${YELLOW}$ARQUIVO${NC} with ${YELLOW}$CXX_VERSION${NC}."
COMPILE_OUTPUT=$(g++ -std=$CXX_VERSION -O2 -Wall -DDEBUG -o "$LETRA" "$ARQUIVO" 2>&1)
COMPILE_STATUS=$?
if [ $COMPILE_STATUS -ne 0 ]; then
    echo -e "${RED}Compilation failed!${NC}"
    echo "$COMPILE_OUTPUT"
    exit 1
fi
# Mostrar avisos de compilação, se houver
if [ ! -z "$COMPILE_OUTPUT" ]; then
    echo "$COMPILE_OUTPUT" | while IFS= read -r line; do
        # Substituir partes específicas para destacar em roxo
        if [[ "$line" == *"warning:"* ]]; then
            line="${line/warning:/${PURPLE}warning:${NC}}"
        fi
        if [[ "$line" == *"-Wsign-compare"* ]]; then
            line="${line/-Wsign-compare/${PURPLE}-Wsign-compare${NC}}"
        fi
        if [[ "$line" == *"<="* || "$line" == *">="* || "$line" == *"=="* ]]; then
            # Encontrar a parte da expressão que contém o operador
            if [[ "$line" =~ ([a-zA-Z0-9_+. ()]+[<>=]{1,2}[a-zA-Z0-9_+. ()]+) ]]; then
                expr="${BASH_REMATCH[1]}"
                line="${line/$expr/${PURPLE}$expr${NC}}"
            fi
        fi
        if [[ "$line" == *"~"* ]]; then
            line="${PURPLE}$line${NC}"
        fi
        echo -e "$line"
    done
fi
# Execute o programa diretamente, sem mensagens adicionais
./"$LETRA"
```

3. Torne os scripts executáveis:

```bash
chmod +x ~/bin/runsamples
chmod +x ~/bin/dbrun
```

> [!IMPORTANT]
> Sem as permissões de execução corretas, os scripts não funcionarão. Não se esqueça de executar o comando `chmod`.

### Configurar o Script Shell

1. Torne o script s.sh executável:

```bash
cd ~/Sublime
chmod +x s.sh
```

### Instalar Pacotes Necessários

```bash
sudo apt install bc time
```

---

## ⚙️ Arquivos de Configuração

### Atualizar o .bashrc

Faça backup do seu arquivo .bashrc atual:

```bash
cp ~/.bashrc ~/.bashrc.bak
```

Em seguida, atualize-o com as configurações necessárias:

```bash
# Adicionar ao final do arquivo ~/.bashrc
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

> [!CAUTION]
> Sempre faça um backup do seu arquivo .bashrc antes de modificá-lo para evitar problemas com sua configuração do terminal.

---

## 🖥️ Configuração do IDE

### Instalar o Package Control no Sublime Text 4

O Package Control é o gerenciador de pacotes do Sublime Text que facilita a instalação de outros plugins e recursos e é geralmente a primeira coisa a se fazer após a instalação do Sublime Text.

1. Abra o Sublime Text 4
2. Pressione `Ctrl + Shift + P` para abrir a paleta de comandos
3. Digite "Install Package Control" e selecione quando aparecer
4. Aguarde a instalação concluir e reinicie o Sublime Text se necessário

Se o método acima não funcionar, você pode:
- Ir até https://packagecontrol.io/installation e copiar o código para instalar o Package Control
- Abrir o console do Sublime Text com `View > Show Console`
- Colar o código e pressionar Enter
- Reiniciar o Sublime Text

> [!NOTE]
> O Package Control é essencial para gerenciar facilmente pacotes no Sublime Text. Ele simplifica a instalação, atualização e remoção de plugins.

### Instalar Pacotes Essenciais

Com o Package Control instalado, você pode adicionar os pacotes recomendados:

1. Pressione `Ctrl + Shift + P` para abrir a paleta de comandos
2. Digite "Package Control: Install Package" e selecione
3. Aguarde o carregamento da lista de pacotes
4. Busque e instale cada um dos seguintes pacotes:

#### LSP (Language Server Protocol)
- **Descrição**: Fornece recursos avançados de IDE como autocomplete, diagnósticos, etc.
- **Repositório**: [LSP - GitHub](https://github.com/sublimelsp/LSP)
- **Documentação**: [LSP - Documentação](https://lsp.sublimetext.io/)

#### ClangAutoComplete
- **Descrição**: Plugin para Sublime Text 3 que oferece autocompleção de membros de estruturas C/C++ ou atributos e métodos de classes
- **Repositório**: [ClangAutoComplete - GitHub](https://github.com/pl-ca/ClangAutoComplete)
- **Documentação**: [README do ClangAutoComplete](https://github.com/pl-ca/ClangAutoComplete#readme)
- **Configuração**: Edite as configurações em `Preferences > Package Settings > ClangAutoComplete > Settings`
- **Alternativa**: O EasyClangComplete é uma alternativa robusta para completar código C/C++ no Sublime Text 3/4 que possui mais recursos e atualizações frequentes

#### SublimeLinter-clang
- **Descrição**: Plugin do SublimeLinter que fornece uma interface para o clang, utilizado para arquivos com sintaxe C/C++
- **Repositório**: [SublimeLinter-clang - GitHub](https://github.com/SublimeLinter/SublimeLinter-clang)
- **Documentação**: [README do SublimeLinter-clang](https://github.com/SublimeLinter/SublimeLinter-clang#readme)
- **Requisitos**: O SublimeLinter principal deve estar instalado para usar este plugin

#### 1337 Color Scheme
- **Descrição**: Um esquema de cores escuro para o Sublime Text
- **Repositório**: [1337 Color Scheme - GitHub](https://github.com/MarkMichos/1337-Scheme)
- **Ativação**: Após instalar, vá em `Preferences > Select Color Scheme` e escolha "1337"

### Instalar e Configurar o Clangd

```bash
# Instalar o Clangd
sudo apt-get install clangd
```

Em seguida, no Sublime Text:

1. Pressione `Ctrl + Shift + P` para abrir a paleta de comandos
2. Digite "LSP Settings" e selecione
3. Adicione a seguinte configuração básica:

```json
{
  "clients": {
    "clangd": {
      "command": ["clangd"],
      "selector": "source.c, source.c++, source.objc, source.objcpp"
    }
  }
}
```

Para uma configuração mais completa, use esta configuração avançada:

```json
{ 
    "clients": 
    { 
        "clangd": { 
            "enabled": true, 
            "command": [ 
                "/usr/bin/clangd", 
                // you may use an absolute path for this clangd executable 
                "-function-arg-placeholders=0", 
                "-header-insertion-decorators=1", 
                "-index" 
            ], 
            "scopes": [ 
                "source.c", 
                "source.c++", 
                "source.objc", 
                "source.objc++" 
            ], 
            "syntaxes": [ 
                "Packages/C++/C.sublime-syntax", 
                "Packages/C++/C++.sublime-syntax", 
                "Packages/Objective-C/Objective-C.sublime-syntax", 
                "Packages/Objective-C/Objective-C++.sublime-syntax" 
            ], 
            "languageId": "cpp",
            "languageId": "c",
            "languageId": "cc"   
        },
        
        "jdtls": {
            "enabled": true,
            "command": [
                "java",
                "-Declipse.application=org.eclipse.jdt.ls.core.id1",
                "-Dosgi.bundles.defaultStartLevel=4",
                "-Declipse.product=org.eclipse.jdt.ls.core.product",
                "-Dlog.level=ALL",
                "-noverify",
                "-Xmx1G",
                "-jar",
                "/home/parallels/jdtls/plugins/org.eclipse.equinox.launcher_1.6.400.v20210924-0641.jar",
                "-configuration",
                "/home/parallels/jdtls/config_linux",
                "-data",
                "/tmp/jdtls-workspace"
            ],
            "scopes": ["source.java"],
            "syntaxes": ["Packages/Java/Java.sublime-syntax"],
            "languageId": "java"
        }
    }
}
```

### 📋 Pré-requisitos para configuração do Java no LSP
1. Instalação do Java JDK
Primeiro, verifique se o Java está instalado:
```bash
java -version
javac -version
```
Se aparecer command not found, instale o JDK:

### Para Java 17 (recomendado)
```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
```

### OU para Java 21
```bash
sudo apt update
sudo apt install openjdk-21-jdk -y
```
Após a instalação, verifique novamente:
```bash
java -version
javac -version
```
2. Descobrir o caminho do Java
```bash
which java
```
Geralmente retorna `/usr/bin/java`. Guarde esse caminho!
### 🛠️ Configuração do LSP (Language Server Protocol)
1. Instalar o pacote `LSP` no Sublime Text

Abra o Sublime Text
`Ctrl+Shift+P` → `Package Control: Install Package`
Procure e instale `LSP`

2. Baixar e configurar o `JDTLS (Java Language Server)`
- Passo 1: Abra o terminal e crie diretório e baixe o servidor
```bash
# Criar pasta para o jdtls
mkdir ~/jdtls
```
### Baixar o language server
```bash
cd ~/Downloads
wget https://download.eclipse.org/jdtls/milestones/1.9.0/jdt-language-server-1.9.0-202203031534.tar.gz
```

### Extrair para a pasta criada
```bash
tar -xzf jdt-language-server-1.9.0-202203031534.tar.gz -C ~/jdtls
```

- Passo 2: Verificar a instalação
```bash
# Verificar se extraiu corretamente
ls ~/jdtls
```
Deve aparecer: `config_linux, plugins, README.md`
- Passo 3: Encontrar o arquivo JAR exato
```bash
ls ~/jdtls/plugins/org.eclipse.equinox.launcher_*.jar
```
Anote o nome completo do arquivo (exemplo: `org.eclipse.equinox.launcher_1.6.400.v20210924-0641.jar`)      

3. Configurar o `LSP` no Sublime Text

No Sublime: `Preferences → Package Settings → LSP → Settings`  
Cole a configuração abaixo, ajustando o nome do `JAR` para o que você encontrou:

```json
{
    "clients": {
        "clangd": {
            "enabled": true,
            "command": [
                "/usr/bin/clangd",
                "-function-arg-placeholders=0",
                "-header-insertion-decorators=1",
                "-index"
            ],
            "scopes": [
                "source.c",
                "source.c++",
                "source.objc",
                "source.objc++"
            ],
            "syntaxes": [
                "Packages/C++/C.sublime-syntax",
                "Packages/C++/C++.sublime-syntax",
                "Packages/Objective-C/Objective-C.sublime-syntax",
                "Packages/Objective-C/Objective-C++.sublime-syntax"
            ],
            "languageId": "cpp"
        },
        "jdtls": {
            "enabled": true,
            "command": [
                "java",
                "-Declipse.application=org.eclipse.jdt.ls.core.id1",
                "-Dosgi.bundles.defaultStartLevel=4",
                "-Declipse.product=org.eclipse.jdt.ls.core.product",
                "-Dlog.level=ALL",
                "-noverify",
                "-Xmx1G",
                "-jar",
                "/home/SEU_USUARIO/jdtls/plugins/org.eclipse.equinox.launcher_VERSAO_AQUI.jar",
                "-configuration",
                "/home/SEU_USUARIO/jdtls/config_linux",
                "-data",
                "/tmp/jdtls-workspace"
            ],
            "scopes": ["source.java"],
            "syntaxes": ["Packages/Java/Java.sublime-syntax"],
            "languageId": "java"
        }
    }
}
```

###Script de Instalação Automática (opcional)  
Crie um `install-jdtls.sh`:   
```bash
#!/bin/bash
# Script de instalação automática do JDTLS

echo "🚀 Instalando JDTLS para Sublime Text..."

# Criar diretório
mkdir -p ~/jdtls

# Baixar JDTLS
cd ~/Downloads
wget https://download.eclipse.org/jdtls/milestones/1.9.0/jdt-language-server-1.9.0-202203031534.tar.gz

# Extrair
tar -xzf jdt-language-server-1.9.0-202203031534.tar.gz -C ~/jdtls

echo "✅ JDTLS instalado com sucesso!"
echo "📝 Agora configure o LSP no Sublime Text"

```

### ⚠️ IMPORTANTE:   
Substitua:

`*SEU_USUARIO*` pelo seu nome de usuário  
`*VERSAO_AQUI*` pelo nome exato do arquivo JAR que você encontrou  

4. Reiniciar o `LSP`
Após salvar a configuração, clique em "Restart" quando aparecer a notificação.

### 🔨 Configuração do Build System (Compilar e Executar)
Criar um `Build System` customizado

No Sublime: `Tools → Build System → New Build System`  
Cole o código abaixo:

```json
{
    "cmd": ["javac", "$file_name", "&&", "java", "$file_base_name"],
    "selector": "source.java",
    "shell": true
}
```
Salve como: `Java.sublime-build`

### Como usar

Crie um arquivo `.java`  
Pressione `Ctrl+B (ou F7)`  
Veja o resultado na parte inferior do Sublime!

### 🐛 Resolução de Problemas
Erro: "java command not found" no `LSP`  
Solução: Use o caminho completo do Java no comando:
```json
"command": [
    "/usr/bin/java",  // Caminho completo em vez de apenas "java"
    "-Declipse.application=org.eclipse.jdt.ls.core.id1",
    // ... resto da configuração
]
```
Erro: `JDTLS` crashando constantemente  
Possíveis soluções:  

Verificar permissões:

```bash
chmod -R 755 ~/jdtls
```
Criar workspace manualmente:

```bash
mkdir -p /tmp/jdtls-workspace
```
Se ainda persistir o erro, tente usar um caminho diferente para o workspace:

```json
"-data",
"/home/SEU_USUARIO/.jdtls-workspace"  // Em vez de /tmp
```
Erro: Build System não funciona  
Se o `Ctrl+B` não funcionar:

Verifique se o Java está no PATH:

```bash
echo $PATH
```
Se necessário, adicione ao `Build System` o caminho completo:

```json
{
    "cmd": ["/usr/bin/javac", "$file_name", "&&", "/usr/bin/java", "$file_base_name"],
    "selector": "source.java",
    "shell": true
}
```
`LSP` não reconhece arquivos Java  
Solução: Certifique-se de que o arquivo tem a extensão `.java` e que está salvo antes de tentar usar o `LSP`.

### ✅ Teste Final
Crie um arquivo `HelloWorld.java`:
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```
Salve o arquivo  
Pressione `Ctrl+B` para compilar e executar  
Deve aparecer `"Hello, World!"` no console do Sublime  

### 📌 Dicas Extras

Para projetos maiores: Considere usar Maven ou Gradle com plugins específicos do Sublime
Alternativa mais simples: Se o JDTLS der muito problema, você pode desabilitar ele e usar apenas o `Build System`  
Performance: Se o Sublime ficar lento, reduza a memória do JDTLS mudando -Xmx1G para -Xmx512M

Se o Sublime não encontrar o clangd, verifique o caminho:

```bash
which clangd
```

Em seguida, atualize a configuração com o caminho completo, se necessário.

### Ativar o Painel de Diagnóstico do LSP

Para obter feedback em tempo real de erros e avisos:

1. Vá para `Tools > LSP > Toggle Diagnostics Panel`
2. Isso mostrará erros e avisos de compilação enquanto você digita

> [!WARNING]
> Sem a configuração adequada do `LSP`, você perderá recursos críticos de detecção de erros em tempo real, o que é crucial para programação competitiva.

---

## 🚀 Guia de Uso

### Referência de Comandos

| Comando | Descrição |
|---------|-------------|
| `runsamples A` | Compilar e executar o problema A com todos os casos de teste |
| `dbrun A` | Compilar e executar o problema A no modo interativo |
| `python3 ~/Sublime/download_prob.py [contest_id] [problem_letter]` | Baixar problemas do Codeforces |
| `python3 ~/Sublime/download_problem.py [url_do_problema]` | Baixar problemas de outras plataformas |

### Fluxo de Trabalho

1. **Download de um novo problema**:
   ```bash
   # Para o Codeforces
   cd ~/Sublime/Codeforces/Codeforces\ Round\ 991\ \(Div.\ 3\)/
   python3 ~/Sublime/gen_cf.py 1234 A  # Contest ID 1234, Problema A
   
   # Para outras plataformas
   python3 ~/Sublime/download_problem.py https://atcoder.jp/contests/abc300/tasks/abc300_a
   ```

2. **Crie um novo arquivo de problema** usando o template:
   ```bash
   cp ~/Sublime/template.cc A.cc
   ```

3. **Edite o arquivo do problema** com sua solução.

4. **Teste sua solução** usando os comandos fornecidos:
   ```bash
   # Executar contra casos de teste
   runsamples A

   # Executar interativamente
   dbrun A
   ```

5. **Depure sua solução** quando necessário.

6. **Submeta sua solução** na plataforma correspondente.

---

## 💡 Dicas e Truques

### 1. Configurar a Extensão Competitive Companion

A Competitive Companion é uma extensão do Chrome essencial para baixar problemas automaticamente dos juízes online.

#### Instalação da Extensão

1. Abra o Chrome e acesse a [Chrome Web Store](https://chrome.google.com/webstore)
2. Pesquise por "Competitive Companion" ou acesse diretamente: [Competitive Companion](https://chrome.google.com/webstore/detail/competitive-companion/cjnmckjndlpiamhfimnnjmnckgghkjbl)
3. Clique em "Adicionar ao Chrome" e confirme a instalação

#### Configuração da Porta

1. Clique no ícone da extensão na barra de ferramentas do Chrome
2. Clique no ícone de engrenagem para abrir as configurações
3. Na seção "Listen on ports", adicione a porta `10046` (mantenha outras portas como 10045 se já estiverem configuradas)
4. Salve as configurações

#### Como Usar

1. Navegue até a página de um problema no site da competição (Codeforces, AtCoder, CodeChef, etc.)
2. Clique no ícone da extensão Competitive Companion na barra de ferramentas
3. A extensão extrairá os dados do problema (título, limites de tempo/memória, exemplos de entrada/saída) e enviará para seu ambiente local
4. Os arquivos de casos de teste serão automaticamente criados no diretório atual com o formato correto (A-1.in, A-1.out, etc.)

> [!TIP]
> A Competitive Companion funciona com a maioria dos juízes online populares, incluindo Codeforces, AtCoder, CodeChef, HackerRank, LeetCode, entre outros. Isso economiza muito tempo na configuração manual de casos de teste.

### 2. Dicas de Depuração

- Use a diretiva `#ifdef DEBUG` no seu código para incluir saídas de depuração que só aparecem no modo de depuração.
- Configure breakpoints no seu código com GDB para depuração interativa.

### 3. Ferramentas para Geração e Teste de Casos

Os seguintes arquivos são incluídos na configuração para facilitar a geração e teste de casos:

#### brute.cpp
Este arquivo implementa uma solução de força bruta para um problema. É útil para verificar a corretude da sua solução otimizada comparando as saídas em casos de teste aleatórios.

Exemplo de um `brute.cpp` para encontrar o segundo menor elemento:
```cpp
// brute.cpp
// solução lenta para encontrar o segundo menor elemento
#include <bits/stdc++.h>
using namespace std;
int main() {
    int n;
    cin >> n;
    vector<int> a(n);
    for(int& x : a) {
        cin >> x;
    }
    for(int x : a) {
        int count_smaller = 0;
        for(int y : a) {
            if(y < x) {
                ++count_smaller;
            }
        }
        if(count_smaller == 1) {
            cout << x;
            return 0;
        }
    }
    assert(false);
}
```

#### gen.cpp
Gera casos de teste aleatórios para testar sua solução e a solução de força bruta.

#### gen_cf.py
Script para baixar casos de teste do Codeforces e formatá-los corretamente para uso com os comandos `runsamples` e `dbrun`.

#### gen_tree.cpp e gen_tree2.cpp
Scripts especializados para gerar árvores aleatórias, úteis para problemas de grafos.

### 4. Template Otimizado para Competições

Template completo que inclui todas as bibliotecas comuns, macros úteis para depuração e estrutura básica para competições:

> [!TIP]
> Use a macro `dbg(variável)` para depuração. Ela imprimirá o nome e o valor da variável no stderr quando compilado com a flag `-DDEBUG`. Por exemplo, `dbg(v)` mostrará `LINE(42) -> (v): {1, 2, 3}` para um vetor v com elementos 1, 2, 3 na linha 42.

### 5. Otimização de Desempenho

- Compile com `-O2` para desempenho ideal (já incluído nos scripts).
- Esteja ciente dos limites de tempo e memória para cada problema.

> [!TIP]
> Para competições com limites de tempo apertados, utilize estruturas de dados e algoritmos mais eficientes, evitando estruturas da STL que podem ser mais lentas em determinados casos.

---

<div align="center">

❤️ **Bom Código e Boa Sorte nas Competições!** ❤️

</div>
