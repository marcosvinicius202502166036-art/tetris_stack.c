# tetris_stack.c
Projeto em C para o desafio Tetris Stack usando pilhas e filas
#include <stdio.h>
#include <stdlib.h>

#define MAX 5  // tamanho da fila e pilha

// Estrutura da fila
typedef struct {
    int itens[MAX];
    int frente, tras;
} Fila;

// Estrutura da pilha
typedef struct {
    int itens[MAX];
    int topo;
} Pilha;

// Funções da fila
void inicializaFila(Fila *f) {
    f->frente = 0;
    f->tras = 0;
}

int filaVazia(Fila *f) {
    return f->frente == f->tras;
}

int filaCheia(Fila *f) {
    return (f->tras + 1) % MAX == f->frente;
}

void enfileirar(Fila *f, int valor) {
    if (filaCheia(f)) {
        printf("Fila cheia! Não é possível adicionar peça %d.\n", valor);
        return;
    }
    f->itens[f->tras] = valor;
    f->tras = (f->tras + 1) % MAX;
}

int desenfileirar(Fila *f) {
    if (filaVazia(f)) {
        printf("Fila vazia!\n");
        return -1;
    }
    int valor = f->itens[f->frente];
    f->frente = (f->frente + 1) % MAX;
    return valor;
}

// Funções da pilha
void inicializaPilha(Pilha *p) {
    p->topo = -1;
}

int pilhaVazia(Pilha *p) {
    return p->topo == -1;
}

int pilhaCheia(Pilha *p) {
    return p->topo == MAX - 1;
}

void empilhar(Pilha *p, int valor) {
    if (pilhaCheia(p)) {
        printf("Pilha cheia! Não é possível reservar peça %d.\n", valor);
        return;
    }
    p->topo++;
    p->itens[p->topo] = valor;
}

int desempilhar(Pilha *p) {
    if (pilhaVazia(p)) {
        printf("Pilha vazia!\n");
        return -1;
    }
    int valor = p->itens[p->topo];
    p->topo--;
    return valor;
}

// Função para mostrar estado atual
void mostrarEstado(Fila *f, Pilha *p) {
    printf("\nFila (peças futuras): ");
    int i = f->frente;
    while (i != f->tras) {
        printf("%d ", f->itens[i]);
        i = (i + 1) % MAX;
    }

    printf("\nPilha (peças reservadas): ");
    for (int j = 0; j <= p->topo; j++) {
        printf("%d ", p->itens[j]);
    }
    printf("\n");
}

int main() {
    Fila fila;
    Pilha pilha;
    int opcao, valor;

    inicializaFila(&fila);
    inicializaPilha(&pilha);

    while (1) {
        printf("\n--- Tetris Stack ---\n");
        printf("1 - Adicionar peça na fila\n");
        printf("2 - Jogar peça (remover da fila)\n");
        printf("3 - Reservar peça (mover da fila para pilha)\n");
        printf("4 - Recuperar peça da pilha para fila\n");
        printf("5 - Mostrar estado\n");
        printf("6 - Sair\n");
        printf("Escolha: ");
        scanf("%d", &opcao);

        switch(opcao) {
            case 1:
                printf("Digite o número da peça: ");
                scanf("%d", &valor);
                enfileirar(&fila, valor);
                break;
            case 2:
                valor = desenfileirar(&fila);
                if (valor != -1)
                    printf("Jogou a peça %d!\n", valor);
                break;
            case 3:
                valor = desenfileirar(&fila);
                if (valor != -1)
                    empilhar(&pilha, valor);
                break;
            case 4:
                valor = desempilhar(&pilha);
                if (valor != -1)
                    enfileirar(&fila, valor);
                break;
            case 5:
                mostrarEstado(&fila, &pilha);
                break;
            case 6:
                printf("Saindo do jogo...\n");
                exit(0);
            default:
                printf("Opção inválida!\n");
        }
    }

    return 0;
}
****
