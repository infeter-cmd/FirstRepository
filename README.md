# FirstRepository
ну типа обучение прохожу...
import pygame
import random
import math
import time

M = 800
pygame.init()
pygame.mixer.init()
screen = pygame.display.set_mode((M,M))
pygame.display.set_caption("Game")
clock = pygame.time.Clock()

def polar(G,r):
    x = r*math.cos(G)
    y = r*math.sin(G)
    return x,y


a = list(range(1000001))
a[1] = 0
lst = []
i = 2
while i <= 1000000:
    if a[i] != 0:
        lst.append(a[i])
        for j in range(i,1000001,i):
            a[j] = 0
    i += 1

points = []
for G in range(0,1000000):
    x,y = polar(G,G)
    points.append([x,y,0])

for h in lst:
    points[h].pop(2)
    points[h].insert(2,1)
    
r = 1.0
E = math.hypot(M,M)
Run = True
show = True
while Run:
    for event in pygame.event.get():
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_ESCAPE:
                Run = False
        if event.type == pygame.MOUSEBUTTONDOWN:
            if event.button == 4:
                if r < 450:r *= 1.1
            if event.button == 5:
                if r > 0.005:r *= 0.9
            if event.button == 1:
                show = not show
    screen.fill((0,0,0))
    pygame.draw.line(screen,(20,20,20),(M//2,0),(M//2,M),2)
    pygame.draw.line(screen,(20,20,20),(0,M//2),(M,M//2),2)
    for x,y,t in points:
        x,y = M//2+round(x*r),M//2-round(y*r)
        if x > 0 and x < M and y > 0 and y < M:
            if t == 0:
                if show:pygame.draw.rect(screen,(0,150,0),(x,y,1,1))
            else:pygame.draw.rect(screen,(150,0,0),(x,y,1,1))
        elif x > E or y > E:
            break
    pygame.display.flip()
pygame.quit()
