public abstract class Produto {

    private static final double MARGEM_PADRAO = 0.2;

    private String descricao;
    protected double precoCusto;
    protected double margemLucro;

    public Produto(String desc, double precoCusto, double margemLucro) {
        this.descricao = desc;
        this.precoCusto = precoCusto;
        this.margemLucro = margemLucro;
    }

    public Produto(String desc, double precoCusto) {
        this.descricao = desc;
        this.precoCusto = precoCusto;
        this.margemLucro = MARGEM_PADRAO;
    }

    public abstract double valorVenda();

    public String toString() {
        return "Produto: " + descricao +
               " | Custo: R$ " + precoCusto +
               " | Margem: " + (margemLucro * 100) + "%" +
               " | Venda: R$ " + valorVenda();
    }
}




public class ProdutoNaoPerecivel extends Produto {

    public ProdutoNaoPerecivel(String desc, double precoCusto, double margemLucro) {
        super(desc, precoCusto, margemLucro);
    }

    public ProdutoNaoPerecivel(String desc, double precoCusto) {
        super(desc, precoCusto);
    }

    public double valorVenda() {
        return precoCusto + (precoCusto * margemLucro);
    }
}



import java.time.LocalDate;
import java.time.temporal.ChronoUnit;

public class ProdutoPerecivel extends Produto {

    private static final double DESCONTO = 0.25;
    private static final int PRAZO_DESCONTO = 7;

    private LocalDate dataDeValidade;

    public ProdutoPerecivel(String desc, double precoCusto, double margemLucro, LocalDate validade) {
        super(desc, precoCusto, margemLucro);
        if (validade.isBefore(LocalDate.now())) {
            System.out.println("Erro: a data de validade nao pode ser anterior a hoje.");
        } else {
            this.dataDeValidade = validade;
        }
    }

    public double valorVenda() {
        LocalDate hoje = LocalDate.now();

        if (dataDeValidade.isBefore(hoje)) {
            System.out.println("Erro: produto vencido, nao pode ser vendido.");
            return 0;
        }

        double preco = precoCusto + (precoCusto * margemLucro);

        long diasRestantes = ChronoUnit.DAYS.between(hoje, dataDeValidade);
        if (diasRestantes <= PRAZO_DESCONTO) {
            preco = preco - (preco * DESCONTO);
        }

        return preco;
    }

    public String toString() {
        return super.toString() + " | Validade: " + dataDeValidade;
    }
}
