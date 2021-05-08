# evolution

from pygame import *
import random

from SettingsGE import *
from platform import node

class GameSprite(sprite.Sprite):
    def __init__(self, player_image, player_x, player_y, player_speed):
        super().__init__()
        self.image = transform.scale(image.load(player_image), (55, 55))
        self.speed = player_speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y

    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

    def createGrid(self):
        col = 0
        row = 50
        cell_num = 0
        self.grid = []
        self.grid.append(node)

        for y in range(44):
            for x in range(74):
                cell_num += 1
                cell = Node(self, [col, row], cell_num)
                if row == 50 or row == 480 or col == 0 or col == 730:
                    cell.edge = True
                # if row == 60 or row  == 470 or col == 10 or col == 720:
                # cell.alive = True
                self.sprites.add(cell)
                col += 10
            row += 10
            col = 0

game = True
finish = True

class Bacteria_1(GameSprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.Surface((20, 20))
        self.image.fill(RED)
        self.x = random.randint(1, 20)
        self.y = random.randint(1, 20)

    def reproduction(self):
        if sprite.collide_rect(bacteria1, bacteria2):
            spawn = random.randint(0, 2)
            if spawn == 0:
                bacteria1.add(bacteria_1)

    def moving(self):
        self.x = self.x + int(random.randint(-1, 1))
        self.y = self.y + int(random.randint(-1, 1))

    def update(self):
        self.x = random.randint(1, 20)
        self.y = random.randint(1, 20)
        while game.grid[rand_n].free != True:
            rand_n = random.randint(0, game.grid.__len__)
        self.node = game.grid[rand_n]
        self.alive = True

class Bacteria_2(GameSprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = pygame.Surface((20, 20))
        self.image.fill(GREEN)
        self.x = random.randint(1, 20)
        self.y = random.randint(1, 20)

    def reproduction(self):
        if sprite.collide_rect(bacteria1, bacteria2):
            spawn = random.randint(0, 2)
            if spawn == 1:
                bacteria2.add(bacteria_2)

    def moving(self):
        self.x = self.x + int(random.randint(-1, 1))
        self.y = self.y + int(random.randint(-1, 1))

    def update(self):
        self.x = random.randint(1, 20)
        self.y = random.randint(1, 20)
        while game.grid[rand_n].free != True:
            rand_n = random.randint(0, game.grid.__len__)
        self.node = game.grid[rand_n]
        self.alive = True

class Node(GameSprite):
    def __init__(self, game, pos, num, color):
        pygame.sprite.Sprite.__init__(self)
        self.game = game
        self.game.node_sprites.add(self)
        self.game.node_list.append(self)

        self.pos = pos
        self.num = num
        self.color = color
        self.free = True

        self.knocked = 0
        self.solver_on = False
        self.checked = False

        self.image = pygame.Surface([20, 20])
        self.image.fill(0)
        self.rect = self.image.get_rect(topleft=self.pos)

        self.point_list = {'north': [self.rect.topleft, self.rect.topright],  # N
                           'east': [self.rect.topright, self.rect.bottomright],  # E
                           'south': [self.rect.bottomright, self.rect.bottomleft],  # S
                           'west': [self.rect.bottomleft, self.rect.topleft]}  # W


window = display.set_mode((WIDTH, HEIGHT))
display.set_caption("Evolution")
window.fill(WHITE)
display.flip()
clock = time.Clock()
beings = sprite.Group()

bacteria1 = sprite.Group()
bacteria2 = sprite.Group()

bacteria_1 = Bacteria_1()
bacteria_2 = Bacteria_2()

running = True
while running:
    clock.tick(FPS)
    for e in event.get():
        if e.type == QUIT:
            running = False


display.update()
clock.tick(FPS)
time.delay(50)
