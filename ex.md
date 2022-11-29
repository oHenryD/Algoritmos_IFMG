import random
import time

#Zera o tabuleiro para uma nova jogada
def inicializaTabuleiro():
    #Preenche o tabuleiro com valores de 0 à 9
    #tabuleiroLocal = [[0,1,2], [3,4,5], [6,7,8]]
    tabuleiroLocal = [[str(i) for i in range(j*3, (j+1)*3)] for j in range(3)]
    return tabuleiroLocal

#Permite que o jogador faça uma jogada
#Lembre-se de verificar se a casa já foi escolhida
def jogadaHumano(tabuleiro):
    imprimeTabuleiro(tabuleiro)
    j = input("Escolha o número da sua jogada: ")
    if j in tabuleiro[0] or j in tabuleiro[1] or j in tabuleiro[2]:
        j = int(j)
        tabuleiro[j//3][j%3] = "*"
        imprimeTabuleiro(tabuleiro)
    else:
        jogadaHumano(tabuleiro)

#A jogada do computador é muito rápida.
#Aqui há um texto simulando que o computador está pensando na jogada...
#Use o sleep da biblioteca time
def imprimeTextoComputador():
    print("O computador está escolhendo sua jogada")
    m = "..."
    for i in m:
        print(i)
        time.sleep(0.1)
    print("Pronto! O computador fez a jogada!")

#Faz o sorteio da jogada do Computador.
#Lembre-se de verificar se a casa já foi escolhida
def jogadaComputador(tabuleiro):
    imprimeTextoComputador()
    jL = random.randint(0,2)
    jC = random.randint(0,2)
    while tabuleiro[jC][jL] in "*C":
        jL = random.randint(0,2)
        jC = random.randint(0,2)

    tabuleiro[jC][jL] = "C"

#Imprime como está o tabuleiro neste momento
def imprimeTabuleiro(tabuleiro):
    print("\n" + "="*30 + "\n")
    for linha in tabuleiro:
        print(" | ".join(linha))
    print("\n")

#Verifica se alguém venceu ou se não há mais jogadas possíveis (Velha)
def verificaTabuleiro(tabuleiro):
    #Verifica diagonais
    if tabuleiro[0][0] == tabuleiro[1][1] and tabuleiro[1][1] == tabuleiro[2][2]:
        return tabuleiro[1][1]
    elif tabuleiro[2][0] == tabuleiro[1][1] and tabuleiro[1][1] == tabuleiro[0][2]:
        return tabuleiro[1][1]
    else:
      #Verifica linhas
      for j in range(3):
          if tabuleiro[j][0] == tabuleiro[j][1] and tabuleiro[j][1] == tabuleiro[j][2]:
              return tabuleiro[j][j]
      #Verifica colunas
      for j in range(3):
          if tabuleiro[0][j] == tabuleiro[1][j] and tabuleiro[1][j] == tabuleiro[2][j]:
              return tabuleiro[j][j]

    return ""

#Verifica quem venceu e emite a mensagem de parabéns!
def vencedor(ganhador, tabuleiro):
    imprimeTabuleiro(tabuleiro)
    if ganhador == "*":
        print("Parabéns, Jogador! Você ganhou!")
    elif ganhador == "C":
        print("Poxa, Jogador! Não deu! Você perdeu para o computador")
    else:
        print("Deu velha!!!")

    time.sleep(0.3)
    op = " "
    while op not in "SN":
        op = input("Sair? (S/N) ").upper()
        if op == "S":
            return op
        elif op == "N":
            print("Ok! Vamos novamente!")
            time.sleep(0.3)

#Controla as jogadas alternadas entre jogador e computador
#até que alguém vença ou não exista mais jogadas possíveis
def controlaJogo(tabuleiro):
    ganhador = ""
    jogada = 0
    while  ganhador == "" and jogada <=8:
        if jogada%2 == 0:
            jogadaHumano(tabuleiro)
            ganhador = verificaTabuleiro(tabuleiro)
        else:
            jogadaComputador(tabuleiro)
            ganhador = verificaTabuleiro(tabuleiro)
        jogada += 1
    return ganhador

#Imprime o menu inicial
def imprimeMenu():
    print("""Escolha uma das opções abaixo
    1 - iniciar o jogada;
    2 - sair""")

#Deve iniciar o jogo ou permitir que o programa seja finalizado
#Deve permitir que várias disputas do jogo sejam feitas, enquanto o
#usuário quiser.
def menu():
    print("Bem-vindo(a) ao Jogo da Velha!")
    tabuleiro = inicializaTabuleiro()
    while True:
        imprimeMenu()
        escolha = input()
        if escolha == '1':
            ganhador = controlaJogo(tabuleiro)
            r = vencedor(ganhador, tabuleiro)
            if r == "S":
                return
            else:
                tabuleiro = inicializaTabuleiro()
        elif escolha == '2':
            return

menu()
