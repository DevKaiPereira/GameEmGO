# Jogo da Adivinhação em Go

Este projeto é um jogo simples de adivinhação escrito em Go. O programa sorteia um número inteiro entre 0 e 100 e o jogador tenta acertar esse número em até 10 tentativas.

Durante o jogo, o programa:

- mostra uma mensagem inicial;
- escolhe um número aleatório;
- lê o chute do usuário;
- compara o número informado com o número sorteado;
- informa se o chute foi maior ou menor;
- mostra uma mensagem de vitória quando o jogador acerta;
- ou informa que o jogador perdeu após 10 tentativas.

---

## Estrutura do projeto

O projeto possui apenas dois arquivos principais:

- `main.go` — contém todo o código do jogo;
- `readme.md` — documentação do projeto.

---

## Código principal

O arquivo principal é o `main.go`. Vamos entender linha por linha.

### 1) Imports

```go
import (
    "bufio"
    "fmt"
    "math/rand/v2"
    "os"
    "strconv"
    "strings"
)
```

Esses imports são bibliotecas que o programa usa:

- `fmt`: imprime mensagens no terminal.
- `bufio`: facilita a leitura de entrada do usuário pelo teclado.
- `math/rand/v2`: gera números aleatórios.
- `os`: trabalha com entrada/saída do sistema, incluindo leitura do teclado.
- `strconv`: converte textos em números.
- `strings`: manipula strings, como remover espaços extras.

---

### 2) Função principal

```go
func main() {
```

A função `main()` é o ponto de entrada do programa. É ela que executa quando você roda o projeto com `go run main.go`.

---

### 3) Mensagem inicial

```go
fmt.Println("Jogo da Adivinhação")
fmt.Println("Um número aleatório será sorteado. Tente acertar. O número é um inteiro entre 0 e 100")
```

Essas linhas mostram ao usuário:

- o nome do jogo;
- a regra do jogo;
- o intervalo do número sorteado, que vai de 0 a 100.

---

### 4) Sorteando o número aleatório

```go
x := rand.Int64N(101)
```

Aqui o programa gera um número aleatório inteiro com o comando `rand.Int64N(101)`.

- `101` significa que o valor vai de 0 até 100.
- `Int64N` indica que o tipo do número gerado é `int64`.
- A variável `x` armazena esse número sorteado.

Exemplo: o valor pode ser `17`, `54`, `99`, etc.

---

### 5) Leitura do teclado

```go
scanner := bufio.NewScanner(os.Stdin)
```

`os.Stdin` representa a entrada padrão do programa, ou seja, o teclado. O scanner vai ler o texto digitado pelo usuário.

Isso permite que o jogo capture cada tentativa do jogador.

---

### 6) Armazenando as tentativas

```go
chutes := [10]int64{}
```

Esse array guarda até 10 chutes válidos do jogador.

- `[10]int64{}` cria um vetor com 10 posições;
- cada posição guarda um valor do tipo `int64`;
- o número 10 representa o limite máximo de tentativas.

---

### 7) Laço de tentativas

```go
for i := range chutes {
```

Esse `for` repete a lógica até percorrer todas as 10 posições do array.

Ou seja, o jogador tem até 10 chances de acertar o número.

---

### 8) Perguntando o chute

```go
fmt.Println("Qual é o seu chute?")
scanner.Scan()
chute := scanner.Text()
chute = strings.TrimSpace(chute)
```

O programa:

- pergunta qual é o chute;
- lê a linha digitada;
- guarda em `chute`;
- remove espaços extras com `strings.TrimSpace()`.

Isso ajuda a evitar erros caso o usuário digite algo como `42` com espaços.

---

### 9) Convertendo o texto em número

```go
chuteInt, err := strconv.ParseInt(chute, 10, 64)
if err != nil {
    fmt.Println("O seu chute tem que ser um número inteiro")
    return
}
```

Aqui o programa tenta converter a entrada do usuário para um número inteiro.

- `strconv.ParseInt` converte a string em `int64`;
- o `10` indica que a base numérica é decimal;
- o `64` indica que o tipo será `int64`;
- `err` recebe o erro caso o texto não seja um número válido.

Se o usuário digitar letras ou texto como `abc`, a conversão falha e o programa termina com a mensagem:

```go
O seu chute tem que ser um número inteiro
```

A instrução `return` encerra a função `main()`.

---

### 10) Comparando o chute

```go
switch {

case chuteInt < x:
    fmt.Println("Você errou. O número sorteado é maior que", chuteInt)
case chuteInt > x:
    fmt.Println("Voce errou. O número sorteado é menor que", chuteInt)
case chuteInt == x:
    fmt.Printf(
        "Parabéns! Você acertou! O número era: %d\n"+
            "Você acertou em %d tentativas. \n"+
            "Essas foram as suas tentativas: %v\n", x, i+1, chutes[:i],
    )

    return
}
```

Aqui está a lógica principal do jogo.

#### `case chuteInt < x`

Se o chute for menor que o número sorteado, o jogo avisa:

- o número sorteado é maior.

#### `case chuteInt > x`

Se o chute for maior que o número sorteado, o jogo avisa:

- o número sorteado é menor.

#### `case chuteInt == x`

Se o usuário acertou, o programa:

- imprime uma mensagem de parabéns;
- mostra qual era o número sorteado;
- informa em qual tentativa ele acertou;
- exibe todas as tentativas anteriores.

A expressão `chutes[:i]` pega apenas os valores guardados até aquela tentativa. Isso faz com que o programa mostre as tentativas válidas antes da vitória.

A instrução `return` encerra o jogo quando o usuário ganha.

---

### 11) Guardando o chute válido

```go
chutes[i] = chuteInt
```

Se o chute não for igual ao número sorteado, ele é guardado no array `chutes`.

Esse código acontece depois do `switch`, porque só queremos salvar tentativas válidas que não venceram o jogo.

Exemplo:

- primeiro chute: 12
- segundo chute: 80
- etc.

Esses valores ficam armazenados em `chutes[0]`, `chutes[1]`, etc.

---

### 12) Quando o jogador não acerta em 10 tentativas

```go
fmt.Printf(
    "Infelizmente, você não acertou o número, que era: %d\n"+
        "Você teve 10 tentativas. \n"+
        "Essas foram as suas tentativas: %v\n", x, chutes)
```

Quando o bloco `for` termina sem que o usuário acerte, o programa exibe:

- que o jogador perdeu;
- qual era o número sorteado;
- que ele fez 10 tentativas;
- todas as tentativas armazenadas no array.

---

## Fluxo do jogo em resumo

1. O programa sorteia um número entre 0 e 100.
2. O usuário digita um chute.
3. O programa valida se o input é um número inteiro.
4. O programa compara o chute com o número sorteado.
5. Se for menor, diz que o número é maior.
6. Se for maior, diz que o número é menor.
7. Se for igual, o usuário venceu.
8. O programa repete até 10 tentativas.
9. Se não acertar, mostra o número correto e as tentativas.

---

## Como executar o projeto

Primeiro, certifique-se de que o Go está instalado na sua máquina.

Depois, abra o terminal no diretório do projeto e execute:

```bash
go run main.go
```

Se tudo estiver certo, o jogo será iniciado e você poderá jogar no terminal.

---

## Exemplo de execução

Um possível fluxo pode ser:

```text
Jogo da Adivinhação
Um número aleatório será sorteado. Tente acertar. O número é um inteiro entre 0 e 100
Qual é o seu chute?
50
Você errou. O número sorteado é maior que 50
Qual é o seu chute?
75
Você errou. O número sorteado é menor que 75
Qual é o seu chute?
60
Parabéns! Você acertou! O número era: 60
Você acertou em 3 tentativas.
Essas foram as suas tentativas: [50 75]
```

---

## Observações importantes

- O programa aceita apenas números inteiros.
- Se o usuário digitar texto, o código encerra imediatamente.
- O valor sorteado fica sempre entre 0 e 100.
- O jogador tem no máximo 10 chances.

---

## Conclusão

Este projeto é um ótimo exemplo de:

- entrada de dados pelo teclado;
- uso de arrays;
- comparações condicionais;
- loops em Go;
- manipulação de strings e conversão de tipos;
- lógica de jogos simples.

É um código pequeno, mas já mostra muitos conceitos importantes para quem está aprendendo Go.

Se você quiser revisar depois, pode voltar para este arquivo e acompanhar o passo a passo do jogo na ordem em que ele é executado.

