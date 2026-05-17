# 🐹 CHIGUIRE QUEST

Un juego en C con mecánicas de aventura y supervivencia. Controla un adorable chiguire (capibara) mientras evitas enemigos y recolectas comida.

---

## 📁 ESTRUCTURA DEL PROYECTO

```
combate/
├── main.c           # Punto de entrada del juego
├── game.c/h         # Lógica principal del juego
├── player.c/h       # Manejo del jugador (chiguire)
├── enemy.c/h        # Sistema de enemigos IA
├── items.c/h        # Gestión de items y power-ups
├── map.c/h          # Mapas y niveles
├── Makefile         # Script de compilación
└── README.md        # Este archivo
```

---

## 🎮 CARACTERÍSTICAS

### Mecánicas de Juego
- ✅ Sistema de movimiento en 4 direcciones
- ✅ Colisiones y física básica
- ✅ Sistema de puntuación y vidas
- ✅ Power-ups temporales (escudo, velocidad)
- ✅ 3 niveles con dificultad progresiva

### Enemigos
1. **Momoy Golem** 🌊 - Tanque de agua, mucho health y defensa
2. **Momoy Normal** 💧 - Proyectiles de agua y daño medio
3. **Momoy Anciano** ⚡ - Mismo estilo que el Momoy normal, pero con golpes más potentes
4. **Peleadora** 🛡️ - Cuerpo a cuerpo, frontal y agresiva
5. **Guerrero** 🪓 - Protege al mago y fuerza la línea defensiva
6. **Mago** 🔮 - Daño alto a distancia, el objetivo principal a vencer
7. **Candrileja** 🔥 - Jefe de fuego, deja quemaduras y hace daño múltiple
8. **Tentáculo** 🦑 - Ataca a un solo objetivo desde los flancos

### Items/Power-ups
- 🌽 **Maíz** - +10 puntos
- 🥕 **Zanahoria** - +25 puntos  
- 🛡️ **Escudo** - Inmunidad temporal (5 seg)
- ⚡ **Velocidad** - Movimiento 2x (3 seg)

### Niveles
1. **Nivel 1: Momoys de Agua** - Tutorial fácil
   - 1 Momoy Golem, 1 Momoy Normal, 1 Momoy Anciano
   - Items de soporte para aprender el combate
   
2. **Nivel 2: Trio de Rivales** - Combate táctico
   - 1 Peleadora, 1 Guerrero, 1 Mago
   - El guerrero protege al mago mientras el mago causa el mayor daño
   
3. **Nivel 3: Boss Candrileja** - Batalla del jefe
   - Candrileja principal + 2 tentáculos
   - Candrileja aplica quemaduras y daño a múltiples objetivos

---

## 🎮 CONTROLES

| Tecla | Acción |
|-------|--------|
| **W** | Mover arriba |
| **S** | Mover abajo |
| **A** | Mover izquierda |
| **D** | Mover derecha |
| **Q** | Salir del juego |

## 📊 FLUJO DE JUEGO

```
MENÚ PRINCIPAL
      ↓
SELECCIONAR NIVEL
      ↓
LOOP DE JUEGO:
  • Leer entrada
  • Actualizar posición
  • Actualizar enemigos
  • Detectar colisiones
  • Renderizar
      ↓
¿VENCER ENEMIGOS O RECOLECTAR ITEMS?
      ↓
SIGUIENTE NIVEL / GAME OVER
```

---

## 🔧 FUNCIONES PRINCIPALES

### Player Management
- `player_create()` - Crea el jugador
- `player_move()` - Mueve el jugador
- `player_take_damage()` - Reduce salud
- `player_activate_shield()` - Activa escudo
- `player_activate_speed()` - Activa velocidad

### Enemy Management
- `enemy_manager_add()` - Agrega enemigo
- `enemy_manager_update()` - Actualiza IA
- `enemy_check_collision()` - Detecta colisión

### Item Management
- `item_manager_add()` - Agrega item
- `item_check_collision()` - Detecta colisión

### Game Core
- `game_create()` - Inicializa juego
- `game_init_level()` - Carga nivel
- `game_update()` - Loop principal
- `game_handle_collisions()` - Gestiona colisiones

---

## 📈 PRÓXIMAS MEJORAS

- [ ] Renderizado con ncurses o SDL2
- [ ] Animaciones de sprites
- [ ] Efectos de sonido
- [ ] Más tipos de enemigos
- [ ] Sistema de habilidades
- [ ] Menú de pausa
- [ ] Highscores/Rankings
- [ ] Modos de juego adicionales

---

## 👥 EQUIPO

- **Enemigos & IA**: Tu equipo (Franyer + Tú)
- **Game Engine**: Estructura modular en C
- **Otros módulos**: Separados para otras personas

---

## 📝 NOTAS DE DESARROLLO

### Patrones de IA de Enemigos

**Momoy Golem (ENEMY_MOMOY_GOLEM)**
```c
// Movimiento lento y defensivo, cambia de dirección cada cierto tiempo
if (pattern_counter % 30 == 0) direction *= -1;
x += direction * speed;
```

**Momoy Normal (ENEMY_MOMOY_NORMAL)**
```c
// Avanza con un patrón ondulado
x += speed;
y += (int)(1.5f * sin(pattern_counter * 0.15f));
```

**Momoy Anciano (ENEMY_MOMOY_ANCIANO)**
```c
// Avanza despacio, pero entra en estado de ataque fuerte periódicamente
if (pattern_counter % 25 == 0) special_cooldown = 3;
if (special_cooldown > 0) x += direction * (speed + 1);
else x += direction * speed;
```

**Peleadora (ENEMY_FIGHTER)**
```c
// Persigue al jugador directamente en X/Y
if (player->x > x) x += speed;
else if (player->x < x) x -= speed;
if (player->y > y) y += speed;
else if (player->y < y) y -= speed;
```

**Guerrero (ENEMY_WARRIOR)**
```c
// Patrulla lenta de frente a la zona del mago
if (pattern_counter % 40 == 0) direction *= -1;
x += direction * speed;
```

**Mago (ENEMY_MAGE)**
```c
// Movimiento errático con cambios de dirección cada cierto tiempo
if (pattern_counter % 18 == 0) direction = rand() % 4;
```

**Candrileja (ENEMY_CANDRILEJA_BOSS)**
```c
// Jefe de fuego que detona con quemaduras periódicamente
if (pattern_counter % 20 == 0) special_cooldown = 5;
```

**Tentáculo (ENEMY_CANDRILEJA_TENTACLE)**
```c
// Avanza arriba y abajo para atacar a un objetivo fijo
if (pattern_counter % 12 == 0) direction *= -1;
y += direction * speed;
```

---

## 📄 LICENCIA

Proyecto estudiantil - Uso libre para fines educativos.

---
