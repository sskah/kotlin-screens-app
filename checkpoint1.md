# 3SIR
# CHECKPOINT 1
# Karine Nascimento Honório da Silva
# RM: 558810
--- 
# Descrição do Projeto
O projeto consiste em um aplicativo desenvolvido em Kotlin que possui uma estrutura simples de navegação entre telas. Ele é composto pelas seguintes telas:
- Login
- Menu
- Pedidos
- Perfil
Cada tela possui um título, botões para navegação entre as telas e cores distintas para facilitar a visualização e diferenciação de cada seção do aplicativo.

---

# Objetivo da prova

Este documento descreve a evolução do projeto solicitado em aula, aplicando a implementação da passagem de parâmetros entre telas utilizando navegação no Jetpack Compose. São apresentadas as alterações realizadas, como a navegação foi configurada e de que forma os parâmetros são enviados e recebidos em cada tela.

---

# Passagem de parâmetro obrigatório na tela de Perfil

Foi implementada a obrigatoriedade de envio do parâmetro nome na navegação para a tela de perfil. A rota foi configurada com o parâmetro diretamente na URL, garantindo que a navegação só ocorra quando um valor for informado. O envio acontece no navigate e o recebimento é feito via arguments, sendo repassado para a função da tela.

A alteração na rota adiciona `/{nome}`, tornando o parâmetro obrigatório. A variável `nome` é recuperada com `getString`, utilizando um valor padrão como fallback. O uso de `nome!!` força o valor como não nulo. Na navegação, foi necessário incluir o valor diretamente na rota. Na função, o parâmetro foi adicionado à assinatura e utilizado no Text com interpolação.

```kotlin id="yq8h1c"
composable(route = "perfil/{nome}") {
    val nome: String? = it.arguments?.getString("nome", "Usuário Genérico")
    PerfilScreen(modifier = Modifier.padding(innerPadding), navController, nome!!)
}
```
```kotlin id="yq8h1c"
onClick = { navController.navigate("perfil/Fulano de Tal") }
```
```kotlin id="yq8h1c"
fun PerfilScreen(
    modifier: Modifier = Modifier,
    navController: NavController,
    nome: String
)

text = "PERFIL - $nome"
```

---

# Passagem de parâmetro opcional na tela de Pedidos

Foi implementado um parâmetro opcional chamado cliente utilizando query parameter. A navegação foi configurada para aceitar esse valor sem obrigatoriedade, definindo um valor padrão. O envio pode ser feito na navegação e o recebimento ocorre via arguments, sendo tratado como opcional na função.

A alteração na rota adiciona `?cliente={cliente}`, indicando que o parâmetro é opcional. O `navArgument` define um `defaultValue`, evitando valores nulos. O parâmetro é recuperado com `getString` sem uso de `!!`. A função foi alterada para receber `String?` e o valor é exibido no Text.

```kotlin id="n0k4tz"
composable(
    route = "pedidos?cliente={cliente}",
    arguments = listOf(navArgument("cliente") {
        defaultValue = "Cliente Genérico"
    })
) {
    PedidosScreen(
        modifier = Modifier.padding(innerPadding),
        navController,
        it.arguments?.getString("cliente")
    )
}
```
```kotlin id="n0k4tz"
fun PedidosScreen(
    modifier: Modifier = Modifier,
    navController: NavController,
    cliente: String?
)
```
```kotlin id="n0k4tz"
text = "PEDIDOS - $cliente"
```

---

# Adição de parâmetro na navegação da tela de Pedidos

Foi implementado o envio do parâmetro opcional cliente na navegação a partir da tela de menu. A navegação foi configurada para incluir o valor diretamente na rota, permitindo que ele seja recebido na tela de pedidos.

A alteração ocorre no `navigate`, onde foi adicionado `?cliente=Cliente XPTO`. Esse valor é anexado à URL e posteriormente recuperado pela rota configurada anteriormente.

```kotlin id="u6g3mw"
onClick = { navController.navigate("pedidos?cliente=Cliente XPTO") }
```

---

# Passagem de múltiplos parâmetros entre telas

Foi implementada a passagem de múltiplos parâmetros obrigatórios, nome e idade, na navegação da tela de perfil. A rota foi configurada com ambos os parâmetros e seus tipos foram definidos. O envio ocorre no navigate respeitando a ordem e o recebimento é feito via arguments.

A alteração na rota adiciona `/{nome}/{idade}`, tornando ambos obrigatórios. O `navArgument` define os tipos (`String` e `Int`). Os valores são recuperados com `getString` e `getInt`, com fallback. O uso de `!!` força valores não nulos. A navegação foi atualizada para enviar os dois parâmetros na ordem correta. A função foi modificada para receber os dois valores e o Text foi atualizado para exibir ambos.

```kotlin id="5o2z7a"
composable(
    route = "perfil/{nome}/{idade}",
    arguments = listOf(
        navArgument("nome") { type = NavType.StringType },
        navArgument("idade") { type = NavType.IntType }
    )
) {
    val nome: String? = it.arguments?.getString("nome", "Usuário Genérico")
    val idade: Int? = it.arguments?.getInt("idade", 0)

    PerfilScreen(
        modifier = Modifier.padding(innerPadding),
        navController,
        nome!!,
        idade!!
    )
}
```
```kotlin id="n0k4tz"
onClick = { navController.navigate("perfil/Fulano de Tal/27") }
```
```kotlin id="n0k4tz"
fun PerfilScreen(
    modifier: Modifier,
    navController: NavController,
    nome: String,
    idade: Int
)

text = "PERFIL - $nome tem $idade anos"
```
