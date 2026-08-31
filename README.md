import java.util.Scanner;

public class BubbleSort {

    public static void bubbleSort(int[] vetor) {

        for (int i = 0; i < vetor.length - 1; i++) {

            for (int j = 0; j < vetor.length - 1 - i; j++) {

                if (vetor[j] > vetor[j + 1]) {

                    int temp = vetor[j];
                    vetor[j] = vetor[j + 1];
                    vetor[j + 1] = temp;
                }
            }
        }
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        System.out.print("Digite a quantidade de números: ");
        int n = sc.nextInt();

        int[] vetor = new int[n];

        System.out.println("Digite os números:");

        for (int i = 0; i < n; i++) {
            vetor[i] = sc.nextInt();
        }

        bubbleSort(vetor);

        System.out.println("Vetor ordenado:");

        for (int i = 0; i < n; i++) {
            System.out.print(vetor[i] + " ");
        }

        sc.close();
    }
}