
static void listarTodosOsProdutos() {
    if (quantosProdutos == 0) {
        System.out.println("Não há produtos cadastrados.");
        return;
    }
    System.out.println("\nPRODUTOS CADASTRADOS:");
    System.out.println("---------------------");
    for (int i = 0; i < quantosProdutos; i++) {
        System.out.println((i + 1) + " - " + produtosCadastrados[i]);
    }
}


static void cadastrarProduto() {
    if (quantosProdutos >= MAX_NOVOS_PRODUTOS) {
        System.out.println("Limite de produtos atingido. Não é possível cadastrar mais.");
        return;
    }
    // pergunta tipo, descrição, preço, margem...
    // tipo 2 → também lê data de validade e cria ProdutoPerecivel
    // tipo 1 → cria ProdutoNaoPerecivel
    // adiciona ao vetor e incrementa quantosProdutos
}


