
import java.nio.charset.Charset;
import java.util.Scanner;
import java.io.File;
import java.io.PrintWriter;
import java.io.IOException;

public class App {

    /** Quantidade máxima de produtos que podem ser armazenados no vetor */
    static final int MAX_NOVOS_PRODUTOS = 10;

    /** Nome do arquivo de dados. O arquivo deve estar localizado na raiz do projeto */
    static String nomeArquivoDados;
    
    /** Scanner para leitura de dados do teclado */
    static Scanner teclado;

    /** Vetor de produtos cadastrados */
    static Produto[] produtosCadastrados;

    /** Quantidade de produtos cadastrados atualmente no vetor */
    static int quantosProdutos = 0;

    /** Gera um efeito de pausa na CLI. Espera por um enter para continuar */
    static void pausa() {
        System.out.println("Digite enter para continuar...");
        teclado.nextLine();
    }

    /** Cabeçalho principal da CLI do sistema */
    static void cabecalho() {
        System.out.println("AEDs II COMÉRCIO DE COISINHAS");
        System.out.println("=============================");
    }
    
    /** Imprime o menu principal, lê a opção do usuário e a retorna (int).
     * @return Um inteiro com a opção do usuário.
    */
    static int menu() {
        cabecalho();
        System.out.println("1 - Listar todos os produtos");
        System.out.println("2 - Procurar e imprimir os dados de um produto");
        System.out.println("3 - Cadastrar novo produto");
        System.out.println("0 - Sair");
        System.out.print("Digite sua opção: ");
        return Integer.parseInt(teclado.nextLine());
    }
    
    /**
     * Lê os dados de um arquivo-texto e retorna um vetor de produtos. Arquivo-texto no formato
     * N (quantidade de produtos) <br/>
     * tipo;descrição;preçoDeCusto;margemDeLucro;[dataDeValidade] <br/>
     * Deve haver uma linha para cada um dos produtos. Retorna um vetor vazio em caso de problemas com o arquivo.
     * @param nomeArquivoDados Nome do arquivo de dados a ser aberto.
     * @return Um vetor com os produtos carregados, ou vazio em caso de problemas de leitura.
     */
    static Produto[] lerProdutos(String nomeArquivoDados) {
        Produto[] produtos = new Produto[MAX_NOVOS_PRODUTOS];
        try (Scanner arquivo = new Scanner(new File(nomeArquivoDados), Charset.forName("UTF-8"))) {
            int n = Integer.parseInt(arquivo.nextLine().trim());
            for (int i = 0; i < n; i++) {
                String linha = arquivo.nextLine();
                produtos[i] = Produto.criarDoTexto(linha);
                quantosProdutos++;
            }
        } catch (Exception e) {
            quantosProdutos = 0;
            return new Produto[MAX_NOVOS_PRODUTOS];
        }
        return produtos;
    }
    
    /** Localiza um produto no vetor de produtos cadastrados, a partir do nome de produto informado pelo usuário, e imprime seus dados. 
     *  A busca não é sensível ao caso. Em caso de não encontrar o produto, imprime uma mensagem padrão */
    static void localizarProdutos() {
        System.out.print("Digite o nome do produto a localizar: ");
        String nome = teclado.nextLine();
        boolean encontrado = false;

        for (int i = 0; i < quantosProdutos; i++) {
            if (produtosCadastrados[i].descricao.equalsIgnoreCase(nome)) {
                System.out.println(produtosCadastrados[i]);
                encontrado = true;
                break;
            }
        }
        if (!encontrado) {
            System.out.println("Produto não encontrado.");
        }
    }
    
    /**
     * Salva os dados dos produtos cadastrados no arquivo csv informado. Sobrescreve todo o conteúdo do arquivo.
     * @param nomeArquivo Nome do arquivo a ser gravado.
     */
    public static void salvarProdutos(String nomeArquivo) {
        try (PrintWriter escritor = new PrintWriter(nomeArquivo, Charset.forName("UTF-8"))) {
            escritor.println(quantosProdutos);
            for (int i = 0; i < quantosProdutos; i++) {
                escritor.println(produtosCadastrados[i].gerarDadosTexto());
            }
        } catch (IOException e) {
            System.out.println("Erro ao salvar os produtos.");
        }
    }
    
    /** Lista todos os produtos cadastrados, numerados, um por linha */
    static void listarTodosOsProdutos() {
    	
    }
    
    /**
     * Rotina para cadastro de um novo produto: pergunta ao usuário o tipo do produto, lê os dados correspondentes,
     * cria o objeto adequado de acordo com o tipo, inclui o produto no vetor.
     */
    static void cadastrarProduto() {
    	
    }  
    
	public static void main(String[] args) {
		teclado = new Scanner(System.in, Charset.forName("UTF-8"));
        nomeArquivoDados = "dadosProdutos.csv";
        produtosCadastrados = lerProdutos(nomeArquivoDados);
        
        int opcao = -1;
      
        do{
            opcao = menu();
            switch (opcao) {
                case 1 -> listarTodosOsProdutos();
                case 2 -> localizarProdutos();
                case 3 -> cadastrarProduto();
            }
            pausa();
        }while(opcao != 0);       

        salvarProdutos(nomeArquivoDados);
        teclado.close();    
    }
}






import java.text.NumberFormat;
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;

public abstract class Produto {
	
	private static final double MARGEM_PADRAO = 0.2;
	protected String descricao;
	protected double precoCusto;
	protected double margemLucro;
	
     /**
     * Inicializador privado. Os valores default, em caso de erro, são:
     * "Produto sem descrição", R$ 0.00, 0.0  
     * @param desc Descrição do produto (mínimo de 3 caracteres)
     * @param precoCusto Preço do produto (mínimo 0.01)
     * @param margemLucro Margem de lucro (mínimo 0.01)
     */
	private void init(String desc, double precoCusto, double margemLucro) {
		
		if ((desc.length() >= 3) && (precoCusto > 0.0) && (margemLucro > 0.0)) {
			descricao = desc;
			this.precoCusto = precoCusto;
			this.margemLucro = margemLucro;
		} else {
			throw new IllegalArgumentException("Valores inválidos para os dados do produto.");
		}
	}
	
     /**
     * Construtor completo. Os valores default, em caso de erro, são:
     * "Produto sem descrição", R$ 0.00, 0.0  
     * @param desc Descrição do produto (mínimo de 3 caracteres)
     * @param precoCusto Preço do produto (mínimo 0.01)
     * @param margemLucro Margem de lucro (mínimo 0.01)
     */
	protected Produto(String desc, double precoCusto, double margemLucro) {
		init(desc, precoCusto, margemLucro);
	}
	
     /**
     * Construtor sem margem de lucro - fica considerado o valor padrão de margem de lucro.
     * Os valores default, em caso de erro, são:
     * "Produto sem descrição", R$ 0.00 
     * @param desc Descrição do produto (mínimo de 3 caracteres)
     * @param precoCusto Preço do produto (mínimo 0.01)
     */
	protected Produto(String desc, double precoCusto) {
		init(desc, precoCusto, MARGEM_PADRAO);
	}
	
     /**
     * Retorna o valor de venda do produto, considerando seu preço de custo e margem de lucro.
     * @return Valor de venda do produto (double, positivo)
     */
	public abstract double valorDeVenda();
	
	/**
     * Descrição, em string, do produto, contendo sua descrição e o valor de venda.
     *  @return String com o formato:
     * [NOME]: R$ [VALOR DE VENDA]
     */
    @Override
	public String toString() {
    	
    	NumberFormat moeda = NumberFormat.getCurrencyInstance();
    	
    	return String.format("NOME: " + descricao + ": " + moeda.format(valorDeVenda()));
	}
    
    /**
     * Igualdade de produtos: caso possuam o mesmo nome/descrição. 
     * @param obj Outro produto a ser comparado 
     * @return booleano true/false conforme o parâmetro possua a descrição igual ou não a este produto.
     */
    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || !(obj instanceof Produto)) return false;
        Produto outro = (Produto) obj;
        return this.descricao.equalsIgnoreCase(outro.descricao);
    }
    
    /**
     * Cria um produto a partir de uma linha de dados em formato texto. A linha de dados deve estar de acordo com a formatação
     * "tipo;descrição;preçoDeCusto;margemDeLucro;[dataDeValidade]"
     * ou o funcionamento não será garantido. Os tipos são 1, para produto não perecível; e 2, para perecível.
     * @param linha Linha com os dados do produto a ser criado.
     * @return Um produto com os dados recebidos
     */
    static Produto criarDoTexto(String linha) {
        String[] campos = linha.split(";");
        int tipo = Integer.parseInt(campos[0].trim());
        String desc = campos[1].trim();
        double precoCusto = Double.parseDouble(campos[2].trim().replace(",", "."));
        double margemLucro = Double.parseDouble(campos[3].trim().replace(",", "."));

        if (tipo == 1) {
            return new ProdutoNaoPerecivel(desc, precoCusto, margemLucro);
        } else {
            DateTimeFormatter formato = DateTimeFormatter.ofPattern("dd/MM/yyyy");
            LocalDate validade = LocalDate.parse(campos[4].trim(), formato);
            return new ProdutoPerecivel(desc, precoCusto, margemLucro, validade);
        }
    }
    	
    /**
     * Gera uma linha de texto a partir dos dados do produto.
     * @return Uma string no formato "tipo;descrição;preçoDeCusto;margemDeLucro;[dataDeValidade]"
     */
    public abstract String gerarDadosTexto();
}





import java.util.Locale;

public class ProdutoNaoPerecivel extends Produto{

	/**
     * Construtor completo. Causa exceção em caso de valores inválidos para os dados do produto.
     * @param desc Descrição do produto (mínimo de 3 caracteres)
     * @param precoCusto Preço de compra do produto (mínimo 0.01)
     * @param margemLucro Margem de lucro para a venda (mínimo 0.01)
     * @throws IllegalArgumentException em caso dos limites acima serem desrespeitados.
     */
	public ProdutoNaoPerecivel(String desc, double precoCusto, double margemLucro) {
		super(desc, precoCusto, margemLucro);
	}
	
	/**
     * Construtor do produto com margem de lucro padrão (20%). Causa exceção em caso de valores inválidos para os dados do produto.
     * @param desc Descrição do produto (mínimo de 3 caracteres)
     * @param precoCusto Preço de compra do produto (mínimo 0.01)
     * @throws IllegalArgumentException em caso dos limites acima serem desrespeitados.
     */
	public ProdutoNaoPerecivel(String desc, double precoCusto) {
		super(desc, precoCusto);
	}

	/**
     * Retorna o valor de venda do produto, considerando seu preço de custo e margem de lucro
     * @return Valor de venda do produto (double, positivo)
     */
	@Override
	public double valorDeVenda() {
		return (precoCusto * (1.0 + margemLucro));
	}

	/**
     * Gera uma linha de texto a partir dos dados do produto. Preço e margem de lucro são formatados com 2 casas decimais.
     * @return Uma string no formato "1;descrição;preçoDeCusto;margemDeLucro"
     */
	@Override
    public String gerarDadosTexto() {
        return String.format(Locale.US, "1;%s;%.2f;%.2f", descricao, precoCusto, margemLucro);
    }
}










import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.util.Locale;

public class ProdutoPerecivel extends Produto{

	/** Desconto para proximidade de validade: 25% */
	private static final double DESCONTO = 0.25;
	
	/** Prazo, em dias, para conceder o desconto por proximidade da validade */
	private static final int PRAZO_DESCONTO = 7;
	
	/** Data de validade do produto. Não pode ser anterior à data da criação ou venda do produto. */
	private LocalDate dataDeValidade;
	
	/**
     * Construtor completo. 
     * Causa exceção em caso de valores inválidos para os dados do produto.
     * @param desc Descrição do produto (mínimo de 3 caracteres)
     * @param precoCusto Preço de compra do produto (mínimo 0.01)
     * @param margemLucro Margem de lucro para a venda (mínimo 0.01)
     * @param validade Data de validade do produto, que deve ser posterior à data atual.
     * @throws IllegalArgumentException em caso dos limites acima serem desrespeitados.
     */
	public ProdutoPerecivel(String desc, double precoCusto, double margemLucro, LocalDate validade) {
		
		super(desc, precoCusto, margemLucro);
		
		if (validade.isBefore(LocalDate.now())) {
			throw new IllegalArgumentException("Data de validade do produto é anterior ao dia de hoje!");
		}
		dataDeValidade = validade;
	}
	
	/**
     * Construtor do produto com margem de lucro padrão (20%). Causa exceção em caso de valores inválidos para os dados do produto.
     * @param desc Descrição do produto (mínimo de 3 caracteres)
     * @param precoCusto Preço de compra do produto (mínimo 0.01)
     * @param validade Data de validade do produto, que deve ser posterior à data atual.
     * @throws IllegalArgumentException em caso dos limites acima serem desrespeitados.
     */
	public ProdutoPerecivel(String desc, double precoCusto, LocalDate validade) {
		
		super(desc, precoCusto);
		
		if (validade.isBefore(LocalDate.now())) {
			throw new IllegalArgumentException("Data de validade do produto é anterior ao dia de hoje!");
		}
		dataDeValidade = validade;
	}

	/**
     * Retorna o valor de venda do produto, considerando seu preço de custo, margem de lucro e
     * dias de validade. Se o prazo de validade estiver a menos de 7 dias, será concedido desconto de 25%.
     * @return Valor de venda do produto (double, positivo)
     */
	@Override
	public double valorDeVenda() {
		
		double precoVenda;
		
		if (dataDeValidade.isBefore(LocalDate.now())) {
			throw new IllegalArgumentException("Data de validade do produto é anterior ao dia de hoje!");
		}
		
		precoVenda = (precoCusto * (1.0 + margemLucro));
				
		if (LocalDate.now().until(dataDeValidade).getDays() <= PRAZO_DESCONTO) {
			precoVenda = precoVenda * (1.0 - DESCONTO);
		}
		
		return precoVenda;
	}
	
	/**
     * Descrição, em string, do produto, contendo sua descrição, o valor de venda e sua data de validade.
     *  @return String com o formato:
     * [NOME]: R$ [VALOR DE VENDA]
     * Válido até [DD/MM/YYYY]
     */
    @Override
    public String toString(){
    	
        DateTimeFormatter formato = DateTimeFormatter.ofPattern("dd/MM/yyyy");
        
        String dados = super.toString();
        dados += "\nVálido até " + formato.format(dataDeValidade);
        
        return dados;
    }
    
    /**
     * Gera uma linha de texto a partir dos dados do produto. Preço e margem de lucro são formatados com 2 casas decimais.
     * Data de validade é formatada no formato dd/mm/aaaa
     * @return Uma string no formato "2;descrição;preçoDeCusto;margemDeLucro;dataDeValidade"
     */
	@Override
    public String gerarDadosTexto() {
        DateTimeFormatter formato = DateTimeFormatter.ofPattern("dd/MM/yyyy");
        return String.format(Locale.US, "2;%s;%.2f;%.2f;%s",
                descricao, precoCusto, margemLucro, formato.format(dataDeValidade));
    }
}
