# ed2025-aimport java.util.Random;

public class JogoDaVelha {
    protected static final int X = 1, O = -1;
    protected static final int VAZIO = 0;
    protected int tabuleiro[][];
    protected int jogador;
    protected int d;

    public JogoDaVelha(int dimensao) {
        limpaTabuleiro();
        this.tabuleiro = new int[dimensao][dimensao];
        this.d = dimensao;
    };

    public void limpaTabuleiro() {
        for (int i = 0; i < d; i++) {
            for (int j = 0; j < d; j++) {
                tabuleiro[i][j] = VAZIO;
            }
        }
        jogador = X;
    };

    public void poePeca(int i, int j) {
        if (i < 0 || i > 4 || j < 0 || j > 4) {
            throw new IllegalArgumentException("Posição inválida");
        }
        if (tabuleiro[i][j] != VAZIO)
            throw new IllegalArgumentException("Posição ocupada");
        tabuleiro[i][j] = jogador;

        jogador = -jogador;
    }

    public int[] gerarLinhaColuna() {
        Random gerador = new Random();
        int linha = gerador.nextInt(0, d);
        int coluna = gerador.nextInt(0, d);
        return new int[] { linha, coluna };
    }

    public int poePecaAutomatico() {
        int[] posicao = gerarLinhaColuna();
        while (tabuleiro[posicao[0]][posicao[1]] != VAZIO) {
            posicao = gerarLinhaColuna();
        }
        tabuleiro[posicao[0]][posicao[1]] = jogador;
        int acabou = vencedor();

        if (acabou == 1) {
            return 1;
        } else if (acabou == -1) {
            return -1;
        } else if (acabou == 0) {
            return 0;
        } 

        jogador = -jogador;
        return 2;
    }

    public boolean descVencedor(int marca) {
        int somaDiagonal1 = 0;
        int somaDiagonal2 = 0;
        for (int i = 0; i < d; i++) {
            int somaLinha = 0;
            for (int j = 0; j < d; j++) {
                somaLinha += tabuleiro[i][j];
                if (i == j) {
                    somaDiagonal1 += tabuleiro[i][j];
                }

                if (i + j == 2) {
                    somaDiagonal2 += tabuleiro[i][j];
                }
            }
            if (somaLinha == marca * d) {
                return true;
            }
        }

        if (somaDiagonal1 == marca * d) {
            return true;
        }

        if (somaDiagonal2 == marca * d) {
            return true;
        }

        for (int j = 0; j < d; j++) {
            int somaColuna = 0;
            for (int i = 0; i < d; i++) {
                somaColuna += tabuleiro[i][j];
            }
            if (somaColuna == marca * d) {
                return true;
            }
        }

        return false;
    }

    public int vencedor() {
        if (descVencedor(X)) {
            return X;
        } else if (descVencedor(O)) {
            return O;
        } else {
            for (int i = 0; i < d; i++) {
                for (int j = 0; j < d; j++) {
                    if (tabuleiro[i][j] == VAZIO) {
                        return 2;
                    }
                }
            }
        }
        return 0;
    }

    public String toString() {
        String s = "";
        for (int i = 0; i < d; i++) {
            for (int j = 0; j < d; j++) {
                switch (tabuleiro[i][j]) {
                    case X:
                        s += "X";
                        break;
                    case O:
                        s += "O";
                        break;
                    case VAZIO:
                        s += " ";
                        break;
                }
                if (j < d - 1)
                    s += "|"; // limite da coluna
            }
            if (i < d - 1) {
                String tracos = "-".repeat(d * 2 - 1); // limite da linha
                s += "\n" + tracos + "\n"; // limite da linha
            }
        }
        return s;
    }

    public int getJogador() {
        return jogador;
    }
}
