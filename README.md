- 👋 Hi, I’m @taiwuwudi
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
taiwuwudi/taiwuwudi is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
下面是一个简单的Python贪吃蛇游戏代码，你可以运行它来玩游戏。这个游戏使用了Pygame库，所以在运行之前请确保你已经安装了Pygame。

```python
import pygame
import time
import random

pygame.init()

# 设置游戏窗口大小和标题
window_width = 800
window_height = 600
game_window = pygame.display.set_mode((window_width, window_height))
pygame.display.set_caption('贪吃蛇')

# 定义颜色
white = (255, 255, 255)
black = (0, 0, 0)
red = (213, 50, 80)
green = (0, 255, 0)
blue = (50, 153, 213)

# 设置游戏速度
clock = pygame.time.Clock()
snake_speed = 20

# 设置蛇的大小
snake_block = 20

# 设置字体
font_style = pygame.font.SysFont(None, 50)

# 定义显示消息的函数
def message(msg, color):
    mesg = font_style.render(msg, True, color)
    game_window.blit(mesg, [window_width / 6, window_height / 3])

# 定义蛇的函数
def snake(snake_block, snake_list):
    for x in snake_list:
        pygame.draw.rect(game_window, green, [x[0], x[1], snake_block, snake_block])

# 定义游戏主循环
def gameLoop():
    game_over = False
    game_close = False

    # 设置蛇的初始位置
    x1 = window_width / 2
    y1 = window_height / 2

    # 设置蛇的移动步长
    x1_change = 0
    y1_change = 0

    # 设置蛇的长度
    snake_list = []
    length_of_snake = 1

    # 设置食物的位置
    foodx = round(random.randrange(0, window_width - snake_block) / 20.0) * 20.0
    foody = round(random.randrange(0, window_height - snake_block) / 20.0) * 20.0

    while not game_over:

        while game_close == True:
            game_window.fill(blue)
            message("You Lost! Press C-Play Again or Q-Quit", red)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        gameLoop()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_change = -snake_block
                    y1_change = 0
                elif event.key == pygame.K_RIGHT:
                    x1_change = snake_block
                    y1_change = 0
                elif event.key == pygame.K_UP:
                    y1_change = -snake_block
                    x1_change = 0
                elif event.key == pygame.K_DOWN:
                    y1_change = snake_block
                    x1_change = 0

        # 判断蛇是否超出边界
        if x1 >= window_width or x1 < 0 or y1 >= window_height or y1 < 0:
            game_close = True

        x1 += x1_change
        y1 += y1_change
        game_window.fill(black)
        pygame.draw.rect(game_window, red, [foodx, foody, snake_block, snake_block])
        snake_head = []
        snake_head.append(x1)
        snake_head.append(y1)
        snake_list.append(snake_head)
        if len(snake_list) > length_of_snake:
            del snake_list[0]

        for x in snake_list[:-1]:
            if x == snake_head:
                game_close = True

        snake(snake_block, snake_list)
        pygame.display.update()

        # 如果蛇吃到了食物
        if x1 == foodx and y1 == foody:
            foodx = round(random.randrange(0, window_width - snake_block) / 20.0) * 20.0
            foody = round(random.randrange(0, window_height - snake_block) / 20.0) * 20.0
            length_of_snake += 1

        clock.tick(snake_speed)

    pygame.quit()
    quit()

gameLoop()



