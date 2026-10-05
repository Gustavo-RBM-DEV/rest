
import java.util.NoSuchElementException;

public class Pilha<E> {

	private Celula<E> topo;
	private Celula<E> fundo;

	public Pilha() {

		Celula<E> sentinela = new Celula<E>();
		fundo = sentinela;
		topo = sentinela;

	}

	public boolean vazia() {
		return fundo == topo;
	}

	public void empilhar(E item) {

		topo = new Celula<E>(item, topo);
	}

	public E desempilhar() {

		E desempilhado = consultarTopo();
		topo = topo.getProximo();
		return desempilhado;

	}

	public E consultarTopo() {

		if (vazia()) {
			throw new NoSuchElementException("Nao há nenhum item na pilha!");
		}

		return topo.getItem();

	}

	/**
	 * Cria e devolve uma nova pilha contendo os primeiros numItens elementos
	 * do topo da pilha atual.
	 * 
	 * Os elementos são mantidos na mesma ordem em que estavam na pilha original.
	 * Caso a pilha atual possua menos elementos do que o valor especificado,
	 * uma exceção será lançada.
	 *
	 * @param numItens o número de itens a serem copiados da pilha original.
	 * @return uma nova instância de Pilha<E> contendo os numItens primeiros elementos.
	 * @throws IllegalArgumentException se a pilha não contém numItens elementos.
	 */
	public Pilha<E> subPilha(int numItens) {

		if (numItens < 0) {
			throw new IllegalArgumentException("O número de itens não pode ser negativo!");
		}

		// Percorre a pilha a partir do topo, sem alterá-la, guardando os itens em uma pilha auxiliar.
		// Na pilha auxiliar, os itens ficam em ordem invertida (o antigo topo fica no fundo).
		Pilha<E> auxiliar = new Pilha<E>();
		Celula<E> atual = topo;

		for (int i = 0; i < numItens; i++) {
			if (atual == fundo) {
				throw new IllegalArgumentException("A pilha não contém " + numItens + " elementos!");
			}
			auxiliar.empilhar(atual.getItem());
			atual = atual.getProximo();
		}

		// Desempilhar da auxiliar e empilhar na nova pilha restaura a ordem original:
		// o topo da subpilha é o mesmo item que está no topo da pilha atual.
		Pilha<E> subPilha = new Pilha<E>();
		while (!auxiliar.vazia()) {
			subPilha.empilhar(auxiliar.desempilhar());
		}

		return subPilha;
	}
}
