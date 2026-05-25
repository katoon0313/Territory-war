import pygame import random import math from dataclasses import dataclass, field

pygame.init()

WIDTH, HEIGHT = 1280, 720 SCREEN = pygame.display.set_mode((WIDTH, HEIGHT)) pygame.display.set_caption("Territory War Prototype") CLOCK = pygame.time.Clock() FONT = pygame.font.SysFont("consolas", 18) BIG_FONT = pygame.font.SysFont("consolas", 28)

MAP_COLS = 24 MAP_ROWS = 14 TILE_SIZE = 48

FACTIONS = [ {"name": "Red Empire", "color": (220, 70, 70)}, {"name": "Blue Dominion", "color": (70, 120, 220)}, {"name": "Green Union", "color": (80, 180, 90)}, ]

@dataclass class Tile: x: int y: int owner: int = -1 population: int = 0 soldiers: int = 0 resources: int = 0 defense: int = 1

def rect(self):
    return pygame.Rect(self.x * TILE_SIZE, self.y * TILE_SIZE, TILE_SIZE, TILE_SIZE)

@dataclass class Faction: faction_id: int name: str color: tuple gold: int = 300 food: int = 300 tech: int = 1 total_power: int = 0

class World: def init(self): self.tiles = [] self.factions = [] self.selected = None self.turn = 1 self.generate_world()

def generate_world(self):
    for faction_id, faction_data in enumerate(FACTIONS):
        self.factions.append(
            Faction(
                faction_id=faction_id,
                name=faction_data["name"],
                color=faction_data["color"],
            )
        )

    for y in range(MAP_ROWS):
        row = []
        for x in range(MAP_COLS):
            tile = Tile(
                x=x,
                y=y,
                owner=random.randint(0, len(FACTIONS) - 1),
                population=random.randint(30, 120),
                soldiers=random.randint(10, 50),
                resources=random.randint(20, 100),
                defense=random.randint(1, 3),
            )
            row.append(tile)
        self.tiles.append(row)

def get_tile(self, mx, my):
    gx = mx // TILE_SIZE
    gy = my // TILE_SIZE

    if 0 <= gx < MAP_COLS and 0 <= gy < MAP_ROWS:
        return self.tiles[gy][gx]
    return None

def draw(self):
    for row in self.tiles:
        for tile in row:
            faction = self.factions[tile.owner]
            rect = tile.rect()

            pygame.draw.rect(SCREEN, faction.color, rect)
            pygame.draw.rect(SCREEN, (20, 20, 20), rect, 1)

            text = FONT.render(str(tile.soldiers), True, (255, 255, 255))
            SCREEN.blit(text, (rect.x + 10, rect.y + 10))

            if self.selected == tile:
                pygame.draw.rect(SCREEN, (255, 255, 0), rect, 3)

def update_economy(self):
    for faction in self.factions:
        faction.total_power = 0

    for row in self.tiles:
        for tile in row:
            faction = self.factions[tile.owner]
            faction.gold += tile.resources // 4
            faction.food += tile.population // 5
            faction.total_power += tile.soldiers + tile.population

            growth = random.randint(0, 3)
            tile.population += growth

            if faction.food > 0:
                tile.soldiers += random.randint(0, 2)
                faction.food -= 1

def ai_turn(self):
    for row in self.tiles:
        for tile in row:
            neighbors = self.get_neighbors(tile)
            enemy_neighbors = [n for n in neighbors if n.owner != tile.owner]

            if enemy_neighbors and tile.soldiers > 25:
                target = random.choice(enemy_neighbors)
                self.attack(tile, target)

def attack(self, attacker, defender):
    attack_power = attacker.soldiers * random.uniform(0.8, 1.3)
    defense_power = defender.soldiers * defender.defense * random.uniform(0.8, 1.2)

    if attack_power > defense_power:
        lost = random.randint(5, 15)
        attacker.soldiers = max(5, attacker.soldiers - lost)

        defender.owner = attacker.owner
        defender.soldiers = max(5, attacker.soldiers // 2)
        defender.population = max(10, defender.population - random.randint(0, 20))
    else:
        attacker.soldiers = max(0, attacker.soldiers - random.randint(10, 25))

def get_neighbors(self, tile):
    dirs = [(-1, 0), (1, 0), (0, -1), (0, 1)]
    result = []

    for dx, dy in dirs:
        nx = tile.x + dx
        ny = tile.y + dy

        if 0 <= nx < MAP_COLS and 0 <= ny < MAP_ROWS:
            result.append(self.tiles[ny][nx])

    return result

def draw_ui(self):
    panel_x = MAP_COLS * TILE_SIZE
    pygame.draw.rect(SCREEN, (28, 28, 35), (panel_x, 0, WIDTH - panel_x, HEIGHT))

    title = BIG_FONT.render("WORLD STATUS", True, (255, 255, 255))
    SCREEN.blit(title, (panel_x + 20, 20))

    y = 80
    for faction in self.factions:
        text = [
            f"{faction.name}",
            f"Gold: {faction.gold}",
            f"Food: {faction.food}",
            f"Power: {faction.total_power}",
        ]

        pygame.draw.rect(SCREEN, faction.color, (panel_x + 15, y, 12, 12))

        for line in text:
            render = FONT.render(line, True, (230, 230, 230))
            SCREEN.blit(render, (panel_x + 40, y))
            y += 22

        y += 20

    turn_text = BIG_FONT.render(f"Turn {self.turn}", True, (255, 255, 0))
    SCREEN.blit(turn_text, (panel_x + 20, HEIGHT - 80))

    if self.selected:
        s = self.selected
        info = [
            f"Tile ({s.x}, {s.y})",
            f"Owner: {self.factions[s.owner].name}",
            f"Population: {s.population}",
            f"Soldiers: {s.soldiers}",
            f"Resources: {s.resources}",
            f"Defense: {s.defense}",
        ]

        iy = HEIGHT - 260
        for line in info:
            render = FONT.render(line, True, (255, 255, 255))
            SCREEN.blit(render, (panel_x + 20, iy))
            iy += 24

world = World()

running = True while running: CLOCK.tick(60) SCREEN.fill((15, 15, 20))

for event in pygame.event.get():
    if event.type == pygame.QUIT:
        running = False

    if event.type == pygame.MOUSEBUTTONDOWN:
        mx, my = pygame.mouse.get_pos()
        selected = world.get_tile(mx, my)
        if selected:
            world.selected = selected

    if event.type == pygame.KEYDOWN:
        if event.key == pygame.K_SPACE:
            world.update_economy()
            world.ai_turn()
            world.turn += 1

        if event.key == pygame.K_a and world.selected:
            neighbors = world.get_neighbors(world.selected)
            enemies = [n for n in neighbors if n.owner != world.selected.owner]

            if enemies:
                target = random.choice(enemies)
                world.attack(world.selected, target)

world.draw()
world.draw_ui()

help_text = [
    "SPACE = Next Turn",
    "A = Attack Random Neighbor",
    "Click Tile = Select",
]

hy = HEIGHT - 120
for line in help_text:
    render = FONT.render(line, True, (255, 255, 255))
    SCREEN.blit(render, (20, hy))
    hy += 22

pygame.display.flip()

pygame.quit()
