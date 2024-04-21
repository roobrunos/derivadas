# derivadas
Analisador de Funções
package AnalisadorDeFuncoes;

import java.util.Scanner;

public class AnalisadorDeFuncoes {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("Digite uma função (exemplo: 2*x^3 + 5*x^2 - 3*x + 7)= ");
        String expressao = scanner.nextLine();

        if (isValida(expressao)) {
            FuncaoMatematica funcao = new FuncaoMatematica(expressao);

            System.out.println("A função inserida foi: " + funcao);

            FuncaoMatematica derivada = funcao.calcularDerivada();

            System.out.println("A derivada da função é: " + derivada);
        } else {
            System.out.println("Digite uma entrada válida.");
        }

        scanner.close();
    }

    public static boolean isValida(String expressao) {
        // Remove espaços em branco e caracteres não relevantes para a validação
        expressao = expressao.replaceAll("\\s+", "");

        // Verifica se a expressão contém 'x' e '*' para multiplicação
        return expressao.contains("x") && expressao.contains("*");
    }
}


package AnalisadorDeFuncoes;

/**
 *
 * @author Rodrigo Bruno <rbruno.10@hotmail.com>
 * @date 21/04/2024
 * @brief Class FuncaoMatematica
 */
class FuncaoMatematica {
    private final String expressao;

    public FuncaoMatematica(String expressao) {
        this.expressao = expressao;
    }

    public FuncaoMatematica calcularDerivada() {
        // Implementação para calcular a derivada de uma função polinomial
        String[] termos = expressao.split("\\s*\\+\\s*|\\s*-\\s*");
        StringBuilder derivada = new StringBuilder();

        for (String termo : termos) {
            termo = termo.trim();
            if (!termo.isEmpty()) {
                int coeficiente = 0;
                int expoente = 0;

                if (termo.contains("x^")) {
                    String[] partes = termo.split("\\*x\\^");
                    coeficiente = partes.length > 0 ? Integer.parseInt(partes[0].trim()) : 1;
                    expoente = partes.length > 1 ? Integer.parseInt(partes[1].trim()) : 1;
                } else if (termo.endsWith("x")) {
                    coeficiente = termo.startsWith("x") ? 1 : Integer.parseInt(termo.replace("x", "").trim());
                    expoente = 1;
                } else {
                    coeficiente = Integer.parseInt(termo);
                    expoente = 0;
                }

                int novoCoeficiente = coeficiente * expoente;
                int novoExpoente = expoente - 1;
                derivada.append(novoCoeficiente).append("*x^").append(novoExpoente).append(" + ");
            }
        }

        if (derivada.length() > 0) {
            derivada.delete(derivada.length() - 3, derivada.length()); // Remover o último "+ "
            return new FuncaoMatematica(derivada.toString());
        } else {
            return new FuncaoMatematica("0"); // Se não foi possível calcular a derivada, retorna 0
        }
    }

    @Override
    public String toString() {
        return expressao;
    }
}
