public abstract class Produto {

    private static final double MARGEM_PADRAO = 0.2;

    private String descricao;
    protected double precoCusto;
    protected double margemLucro;

    private void init(String desc, double precoCusto, double margemLucro) {
        this.descricao = desc;
        this.precoCusto = precoCusto;
        this.margemLucro = margemLucro;
    }

    protected Produto(String desc, double precoCusto, double margemLucro) {
        init(desc, precoCusto, margemLucro);
    }

    protected Produto(String desc, double precoCusto) {
        init(desc, precoCusto, MARGEM_PADRAO);
    }

    public abstract double valorVenda();

    public String getDescricao() {
        return descricao;
    }

    public double getPrecoCusto() {
        return precoCusto;
    }

    public double getMargemLucro() {
        return margemLucro;
    }

    @Override
    public String toString() {
        return String.format(
            "Produto: %s | Preco de custo: R$ %.2f | Margem de lucro: %.0f%% | Preco de venda: R$ %.2f",
            descricao, precoCusto, margemLucro * 100, valorVenda());
    }
}




public class ProdutoNaoPerecivel extends Produto {

    public ProdutoNaoPerecivel(String desc, double precoCusto, double margemLucro) {
        super(desc, precoCusto, margemLucro);
    }

    public ProdutoNaoPerecivel(String desc, double precoCusto) {
        super(desc, precoCusto);
    }

    @Override
    public double valorVenda() {
        return precoCusto * (1 + margemLucro);
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
            throw new IllegalArgumentException(
                "A data de validade nao pode ser anterior ao dia atual.");
        }
        this.dataDeValidade = validade;
    }

    @Override
    public double valorVenda() {
        LocalDate hoje = LocalDate.now();
        if (dataDeValidade.isBefore(hoje)) {
            throw new IllegalStateException(
                "Produto fora da data de validade, nao pode ser vendido.");
        }
        double preco = precoCusto * (1 + margemLucro);
        long diasRestantes = ChronoUnit.DAYS.between(hoje, dataDeValidade);
        if (diasRestantes <= PRAZO_DESCONTO) {
            preco = preco * (1 - DESCONTO);
        }
        return preco;
    }

    public LocalDate getDataDeValidade() {
        return dataDeValidade;
    }

    @Override
    public String toString() {
        return super.toString() + " | Validade: " + dataDeValidade;
    }
}
