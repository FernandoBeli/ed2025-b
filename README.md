# ed2025-bimport java.util.Scanner;

public class App {
    public static void main(String[] args) throws Exception {
        Scanner sc = new Scanner(System.in);
        boolean jogarNovamente = true;
        
        while (jogarNovamente) {
            System.out.println("Bem-vindo ao Jogo da Velha!");
            System.out.println("Escolha o tamanho do tabuleiro: ");
            int dimensao = sc.nextInt();
            if (dimensao % 2 == 0) {
                System.out.println("O tamanho do tabuleiro deve ser impar. Tente novamente.");
                continue;
            }
            JogoDaVelha jogo2 = new JogoDaVelha(dimensao);
            int acabou = jogo2.poePecaAutomatico();
            
            while (acabou == 2) {
                System.out.println(jogo2.toString());
                System.out.println("Vez do jogador " + (jogo2.getJogador() == 1 ? "X" : "O"));
                System.out.println("\n");
                acabou = jogo2.poePecaAutomatico();
            }
            
            System.out.println(jogo2.toString());
            
            if (acabou == 1) {
                System.out.println("Jogador X venceu!");
            } else if (acabou == -1) {
                System.out.println("Jogador O venceu!");
            } else if (acabou == 0) {
                System.out.println("Empate!");
            }
            
            int resposta = -1;
            do {
                System.out.println("Deseja jogar novamente? (1-Sim/0-Não): ");
                try {
                    resposta = sc.nextInt();
                    if (resposta != 0 && resposta != 1) {
                        System.out.println("Opção inválida. Digite 1 para Sim ou 0 para Não.");
                    }
                } catch (Exception e) {
                    System.out.println("Entrada inválida. Digite apenas 0 ou 1.");
                    sc.nextLine(); // Limpa o buffer do scanner1
                }
            } while (resposta != 0 && resposta != 1);
            
            jogarNovamente = (resposta == 1);
        }
        
        System.out.println("Obrigado por jogar!");
        sc.close();
    }
}
