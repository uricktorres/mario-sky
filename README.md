# mario-sky

# Biblioteca PyGame
import pygame
# Biblioteca para geracao de numeros pseudoaleatorios
import random
# Modulo da biblioteca PyGame que permite o acesso as teclas utilizadas
from pygame.locals import *

pygame.init()

import pygame.mixer 

import pygame.sprite

import pygame

pygame.init()

screen_width = 800
screen_height = 600

screen = pygame.display.set_mode((screen_width, screen_height))
start_screen = pygame.image.load('inicio.png') 
start = True

while start:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            quit()
        if event.type == pygame.KEYDOWN:
            start = False
    screen.blit(start_screen, (0, 0))
    pygame.display.update()

# Classe que representar o jogador
class Player(pygame.sprite.Sprite):
    def __init__(self):
        super(Player, self).__init__()
        image = pygame.image.load("mario.png")
        scaled_image = pygame.transform.scale(image, (image.get_width() / 9, image.get_height() / 9))
        self.surf = pygame.Surface((75, 25))
        self.surf = scaled_image
        self.rect = self.surf.get_rect()


    # Determina acao de movimento conforme teclas pressionadas
    def update(self, pressed_keys):
        if pressed_keys[K_UP]:
            self.rect.move_ip(0, -5)
        if pressed_keys[K_DOWN]:
            self.rect.move_ip(0, 5)
        if pressed_keys[K_LEFT]:
            self.rect.move_ip(-5, 0)
        if pressed_keys[K_RIGHT]:
            self.rect.move_ip(5, 0)

        # Mantem o jogador nos limites da tela do jogo
        if self.rect.left < 0:
            self.rect.left = 0
        elif self.rect.right > 800:
            self.rect.right = 800
        if self.rect.top <= 0:
            self.rect.top = 0
        elif self.rect.bottom >= 600:
            self.rect.bottom = 600

# Classe que representa os inimigos
class Enemy(pygame.sprite.Sprite):
    def __init__(self):
        image_inimigo = pygame.image.load("inimigo_do_mario.png")
        scaled_image_inimigo = pygame.transform.scale(image_inimigo, (image_inimigo.get_width() / 9, image_inimigo.get_height() / 9))

        super(Enemy, self).__init__()
        self.surf = scaled_image_inimigo #Definicao do retangulo
        #self.surf.fill((255,255,255)) #Preenche com cor branca (RGB)
        self.rect = self.surf.get_rect( #Coloca na extrema direita (entre 820 e 900) e sorteia sua posicao em relacao a coordenada y (entre 0 e 600)
            center=(random.randint(820, 900), random.randint(0, 600))
        )
        self.speed = random.uniform(1, 5) #Sorteia sua velocidade, entre 1 e 15

    # Funcao que atualiza a posiçao do inimigo em funcao da sua velocidade e termina com ele quando ele atinge o limite esquerdo da tela (x < 0)
    def update(self):
        self.rect.move_ip(-self.speed, 0)
        if self.rect.right < 0:
            self.kill()


# Inicializa pygame
pygame.init()

#criar uma musica


# Cria a tela com resolução 800x600px
screen = pygame.display.set_mode((800, 600))

# Cria um evento para adicao de inimigos
ADDENEMY = pygame.USEREVENT + 1
pygame.time.set_timer(ADDENEMY, 500) #Define um intervalo para a criacao de cada inimigo (milisegundos)

# Cria o jogador (nosso retangulo)
player = Player()

# Define o plano de fundo, com a cor preta (RGB)
# Define o plano de fundo
x=800
y=600
screen = pygame.display.set_mode((x,y))
pygame.display.set_caption("Mario SKY")
background = pygame.image.load ("zyro-image.png")
background = pygame.transform.scale(background, (x, y))

enemies = pygame.sprite.Group() #Cria o grupo de inimigos
all_sprites = pygame.sprite.Group() #Cria o grupo de todos os Sprites
all_sprites.add(player) #Adicionar o player no grupo de todos os Sprites

running = True #Flag para controle do jogo

while running:
    #Laco para verificacao do evento que ocorreu
    for event in pygame.event.get():
        if event.type == KEYDOWN:
            if event.key == K_ESCAPE: #Verifica se a tecla ESC foi pressionada
                running = False
        elif event.type == QUIT: #Verifica se a janela foi fechada
            running = False
        elif(event.type == ADDENEMY): #Verifica se e o evento de criar um inimigo
            new_enemy = Enemy() #Cria um novo inimigo
            enemies.add(new_enemy) #Adiciona o inimigo no grupo de inimigos
            all_sprites.add(new_enemy) #Adiciona o inimigo no grupo de todos os Sprites
    screen.blit(background, (0, 0)) #Atualiza a exibicao do plano de fundo do jogo (neste caso nao surte efeito)
    pressed_keys = pygame.key.get_pressed() #Captura as as teclas pressionadas
    player.update(pressed_keys) #Atualiza a posicao do player conforme teclas usadas
    enemies.update() #Atualiza posicao dos inimigos
    for entity in all_sprites:
        screen.blit(entity.surf, entity.rect) #Atualiza a exibicao de todos os Sprites

    if pygame.sprite.spritecollideany(player, enemies): #Verifica se ocorreu a colisao do player com um dos inimigos
        player.kill() #Se ocorrer a colisao, encerra o player

    pygame.display.flip() #Atualiza a projecao do jogo

screen_width = 800
screen_height = 600

screen = pygame.display.set_mode((screen_width, screen_height))
game_over_screen = pygame.image.load('fim.png') 
game_over = True

while game_over:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            quit()
        if event.type == pygame.KEYDOWN and event.key == pygame.K_ESCAPE:
            game_over = False
    screen.blit(game_over_screen, (0, 0))
    pygame.display.update()

