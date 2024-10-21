Practical No 1
step up for practical 1(a) and 1(b) both is same
Step 1: Create new project, and select “Windows Forms Application”, select .NET Framework as 2.0 in Visuals C#.
•	Open visual studio 
•	Create a new project
•	Select C# as the language, and Windows as the platform for desktop applications.     
•	Download the framework if not already there to support the project 
 •	Choose the framework version as 2.0 
Step 2 : Click on properties Click on open click on build Select Platform Target and Select x86.
Step 3: Click on View Code of Form 1.	
Step 4: Go to Solution Explorer, right click on project name, and select Add Reference. 
Click on Browse and select the given .dll files which are “Microsoft.DirectX”, “Microsoft.DirectX.Direct3D”,
and “Microsoft.DirectX.DirectX3DX”.
•	Select the dll files from 
C:\Windows\Microsoft.NET\DirectX for Managed Code\1.0.2902.0 
•	Select those references into project
Step 5: Go to Properties Section of Form, select Paint in the Event List and enter as From1_Paint
Step 6: Edit the Form’s C# code file.
•	Ensure the namespace remains default when copying and pasting code.
using Microsoft.DirectX.Direct3D;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Text;
using System.Windows.Forms;

namespace Practical_1_A_
{
    public partial class Form1 : Form
    {
        Microsoft.DirectX.Direct3D.Device device;

        public Form1()
        {
            InitializeComponent();
            InitDevice();
            this.Paint += new PaintEventHandler(Form1_Paint); // Attach the Paint event handler
        }

        private void InitDevice()
        {
            PresentParameters pp = new PresentParameters(); // Create object
            pp.Windowed = true;
            pp.SwapEffect = SwapEffect.Discard;
            device = new Device(0, DeviceType.Hardware, this, CreateFlags.HardwareVertexProcessing, pp);
        }

        private void Render()
        {
            device.Clear(ClearFlags.Target, Color.Blue, 0, 1);
            device.Present();
        }

        private void Form1_Paint(object sender, PaintEventArgs e)
        {
            Render();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            // Form load logic if any
        }
    }
}

 
Step 7: When you run this code you get exception for LoaderLock. To solve this exception, 
go in exception thrown window, open exception setting, select and expand managed debugging assistant , and uncheck the loader lock option, then run your program.
•	Exception
 
•	Open exception settings
 
•	select and expand managed debugging assistant
 
•	Scroll down and Uncheck the loader lock option
 				 
•	Run the program



practical 1(b)
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Text;
using System.Windows.Forms;
using Microsoft.DirectX;
using Microsoft.DirectX.Direct3D;

namespace Practical1_B_
{
    public partial class Form1 : Form
    {
        Microsoft.DirectX.Direct3D.Device device;
        Microsoft.DirectX.Direct3D.Texture Texture;
        Microsoft.DirectX.Direct3D.Font font;
        public Form1()
        {
            InitializeComponent();
            InitDevice();
            InitFont();
            LoadTexture();
        }
        private void InitFont()
        {
            System.Drawing.Font f = new System.Drawing.Font("Arial", 16f, FontStyle.Regular);
            font = new Microsoft.DirectX.Direct3D.Font(device, f);
        }
        private void LoadTexture()
        {
            Texture = TextureLoader.FromFile(device, @"C:\Users\rugve\Downloads\Desert.jpg"); enter your image path address

        }
        private void InitDevice()
        {
            PresentParameters pp = new PresentParameters();
            pp.Windowed = true;
            pp.SwapEffect = SwapEffect.Discard;
            device = new Device(0, DeviceType.Hardware, this, CreateFlags.HardwareVertexProcessing, pp);

        }
        private void Render()
        {
            device.Clear(ClearFlags.Target, Color.CornflowerBlue, 0, 1);
            device.BeginScene();
            using (Sprite s = new Sprite(device))
            {
                s.Begin(SpriteFlags.AlphaBlend);
                s.Draw2D(Texture, new Rectangle(0, 0, 0, 0), new Rectangle(0, 0, device.Viewport.Width,
                device.Viewport.Height), new Point(0, 0), 0f, new Point(0, 0), Color.White);
                font.DrawText(s, "hello", new Point(0, 0), Color.White);
                s.End();
            }
            device.EndScene();
            device.Present();
        }
        private void Form1_Paint(object sender, PaintEventArgs e)
        {
            Render();
        }
        private void Form1_Load(object sender, EventArgs e)
        {
        }
    }
}



Practical 3: Develop Snake Game using pygame.
Cmd installation:
pip install pygame
code;
import pygame
import random
import sys

# Initialize pygame
pygame.init()

# Constants

WIDTH, HEIGHT = 500, 500
CELL_SIZE = 20
FOOD_SIZE = CELL_SIZE
DELAY = 100
SNAKE_COLOR = (0, 255, 0)
FOOD_COLOR = (255, 0, 0)
BG_COLOR = (0, 0, 0)

# Directions
offsets = {
    "up": (0, -CELL_SIZE),
    "down": (0, CELL_SIZE),
    "left": (-CELL_SIZE, 0),
    "right": (CELL_SIZE, 0)
}

# Initialize screen
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Snake Game")

# Clock for controlling frame rate
clock = pygame.time.Clock()

def reset():
    global snake, snake_direction, food_pos
    snake = [[100, 100], [80, 100], [60, 100], [40, 100]]
    snake_direction = "right"
    food_pos = get_random_food_pos()
    move_snake()

def move_snake():
    global snake_direction
    new_head = snake[0].copy()
    new_head[0] += offsets[snake_direction][0]
    new_head[1] += offsets[snake_direction][1]

    if new_head in snake or new_head[0] < 0 or new_head[0] >= WIDTH or new_head[1] < 0 or new_head[1] >= HEIGHT:
        reset()
    else:
        snake.insert(0, new_head)
        if not food_collision():
            snake.pop()
    
    screen.fill(BG_COLOR)
    for segment in snake:
        pygame.draw.rect(screen, SNAKE_COLOR, pygame.Rect(segment[0], segment[1], CELL_SIZE, CELL_SIZE))
    pygame.draw.rect(screen, FOOD_COLOR, pygame.Rect(food_pos[0], food_pos[1], FOOD_SIZE, FOOD_SIZE))
    pygame.display.flip()
    clock.tick(1000 // DELAY)

def food_collision():
    global food_pos
    if snake[0] == list(food_pos):  # Fixed distance calculation with direct position check
        food_pos = get_random_food_pos()
        return True
    return False

def get_random_food_pos():
    x = random.randint(0, (WIDTH - CELL_SIZE) // CELL_SIZE) * CELL_SIZE
    y = random.randint(0, (HEIGHT - CELL_SIZE) // CELL_SIZE) * CELL_SIZE
    return (x, y)

def change_direction(new_direction):
    global snake_direction
    if new_direction == "up" and snake_direction != "down":
        snake_direction = "up"
    elif new_direction == "down" and snake_direction != "up":
        snake_direction = "down"
    elif new_direction == "left" and snake_direction != "right":
        snake_direction = "left"
    elif new_direction == "right" and snake_direction != "left":
        snake_direction = "right"

# Main loop
reset()

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP:
                change_direction("up")
            elif event.key == pygame.K_DOWN:
                change_direction("down")
            elif event.key == pygame.K_LEFT:
                change_direction("left")
            elif event.key == pygame.K_RIGHT:
                change_direction("right")
    
    move_snake()


prcatical 4
step:1 click on Assets and import the image you wanna scroll and click on inamge
and stes it textype type (Default or texture) and change wrap mode repeat
step2: go on Gameobject click on 3D Object select Quad and reamin Quad as background and adjust the size of Quad 
step3: go on GameObject click on light and select direction light 
Step4: drag image on unity you wanna scroll 
step5: go on inspector click on background and insert component by add component click on add newscript 
and create newscript as scroll and click on script and tyoe code 
code:
using UnityEngine;
using System.Collections;

public class Scroll : MonoBehaviour
{
    public float speed = 0.5f;  // Added missing semicolon

    void Start()  // Corrected capitalization of 'Start'
    {
    }

    void Update()  // Corrected capitalization of 'Update'
    {
        Vector2 offset = new Vector2(Time.time * speed, 0);  // Corrected capitalization of 'Vector2'
        GetComponent<Renderer>().material.mainTextureOffset = offset;  // 'renderer' changed to 'GetComponent<Renderer>()'
    }
}

Step6: then attach code on unity and go back on unity and run 






practical 5
Create 2D Target Shooting Game
import pygame
import random

# Screen
pygame.init()
width, height = 800, 600
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("Shooting Game")

# Colors
white = (255, 255, 255)
red = (255, 0, 0)
blue = (0, 0, 255)

# Character
character_size = 50
character_speed = 5
character = pygame.Rect(width // 2 - character_size // 2, height - character_size,
                        character_size, character_size)

# Bullets (triangles) properties
bullet_size = 10
bullets = []

# Enemy (circle) properties
enemy_radius = 20
enemies = []

# Initialize score
score = 0

# Clock for controlling frame rate
clock = pygame.time.Clock()

# Main game loop
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE:
                bullet = pygame.Rect(character.centerx - bullet_size // 2, character.top,
                                     bullet_size, bullet_size)
                bullets.append(bullet)

    # Move bullets
    for bullet in bullets[:]:
        bullet.y -= 10
        if bullet.top < 0:
            bullets.remove(bullet)

    # Spawn enemies
    if random.randint(1, 100) <= 2:
        enemy_x = random.randint(enemy_radius, width - enemy_radius)
        enemy = pygame.Rect(enemy_x - enemy_radius, 0, enemy_radius * 2,
                            enemy_radius * 2)
        enemies.append(enemy)

    # Move enemies
    for enemy in enemies[:]:
        enemy.y += 5
        if enemy.top > height:
            enemies.remove(enemy)

    # Move character
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and character.left > 0:
        character.x -= character_speed
    if keys[pygame.K_RIGHT] and character.right < width:
        character.x += character_speed

    # Check for collisions between bullets and enemies
    for bullet in bullets[:]:
        for enemy in enemies[:]:
            if bullet.colliderect(enemy):
                score += 1
                if bullet in bullets:
                    bullets.remove(bullet)
                if enemy in enemies:
                    enemies.remove(enemy)

    # Check for character collision with enemies
    for enemy in enemies[:]:
        if character.colliderect(enemy):
            running = False

    # Clear the screen
    screen.fill(white)

    # Draw bullets
    for bullet in bullets:
        pygame.draw.polygon(screen, blue, [(bullet.left, bullet.bottom),
                                           (bullet.centerx, bullet.top),
                                           (bullet.right, bullet.bottom)])

    # Draw character
    pygame.draw.rect(screen, red, character)

    # Draw enemies
    for enemy in enemies:
        pygame.draw.circle(screen, red, enemy.center, enemy_radius)

    # Display score
    font = pygame.font.Font(None, 36)
    score_text = font.render(f"Score: {score}", True, red)
    screen.blit(score_text, (10, 10))

    # Update display
    pygame.display.flip()

    # Limit frame rate to 60 FPS
    clock.tick(60)

# Game over display
font = pygame.font.Font(None, 72)
game_over_text = font.render("Game Over", True, red)
screen.blit(game_over_text, (width // 2 - game_over_text.get_width() // 2,
                             height // 2 - game_over_text.get_height() // 2))
pygame.display.flip()

# Wait for a few seconds before closing the game
pygame.time.wait(3000)

# Clean up
pygame.quit()











Practical 6: Create Camera Shake Effect in Unity
Following are the steps to create a Camera Shake Effect in Unity:
STEP 1 : Start Unity
STEP 2 : Create new project 2D. Add project name
STEP 3 : Add folders such as Sprites, Scenes, Scripts
STEP 4 : Add Assets by right-click; Import images inside sprites folder
STEP 5 : Import the Following images : Background and UFO
STEP 6 : Drag the assets to the Hierarchy; Click and hold asset and drag under MainCamera
SImilarly, Drag and add the UFO under the background
STEP 7 : Adjust the MainCamera by changing the size
STEP 8 : Select the UFO from Hierarchy.Add a sorting layer by creating UFO layer, and select the layer. Update the following settings for UFO. Add a circle collider and adjust the radius. Adjust gravity Scale clicking on Add Component and typing in Rigidbody2D in the search field.
STEP 9 : Select the Background from Hierarchy. Add a box collider from Add component > Box Collider 2D. Do this step 4 times for each border of the box collider.  
 Add a script and set in inspector script set the Shake Frequency as 5 which set the intensity of the Shake Effect
using UnityEngine;

public class CameraShakeController : MonoBehaviour
{
    public Transform cameraTransform; // Reference to the camera transform (assigned in Inspector)
    private Vector3 _originalPosOfCam; // Stores the original position of the camera
    public float shakeFrequency = 0.5f; // Assign a default value or set it in the Inspector

    void Start()
    {
        // Ensure the cameraTransform is set
        if (cameraTransform == null)
        {
            cameraTransform = transform; // Fallback to the transform of the object this script is attached to
        }

        // Store the original position of the camera
        _originalPosOfCam = cameraTransform.position;
    }

    void Update()
    {
        // Check if 'S' is pressed and hold for camera shake
        if (Input.GetKey(KeyCode.S))
        {
            ShakeCamera(); // Call the renamed method
        }
        // Stop shaking when 'S' is released
        else if (Input.GetKeyUp(KeyCode.S))
        {
            StopShake();
        }
    }

    private void ShakeCamera() // Renamed method
    {
        // Shake the camera by moving it to a random position within a sphere
        cameraTransform.position = _originalPosOfCam + Random.insideUnitSphere * shakeFrequency;
    }

    private void StopShake()
    {
        // Reset the camera to its original position
        cameraTransform.position = _originalPosOfCam;
    }
}

step ; press key S to run 




 




Practical 7: Create Snowfall Particle effect in Unity
We can achieve this effect using Unity's Particle System component. Following is a step- by-step guide on how to create a basic snowfall particle effect:

1. Create a New Particle System:
• In the Unity Editor, select the GameObject where you want to add the snowfall effect.
• Go to the menu bar and select GameObject > Effects > Particle System.
This will create a new Particle System component attached to the selected GameObject.

2. Configure the Particle System:
• In the Inspector window, you'll see the Particle System component's settings.
Adjust these settings to create a snowfall effect:
• Duration: Set this to a value that suits the length of your snowfall scene. For an ongoing snowfall, you can set it to a high value or loop the system.
• Start Lifetime: This determines how long each snowflake particle will stay on screen. Set it to a value that makes the snowflakes fall for an appropriate amount of time (e.g., 5 seconds).
• Start Speed: Adjust the initial speed of the snowflakes. Typically, a small value like 1-5 units per second will work.
• Start Size: Set the size of the snowflakes. They should be small, like 0.05 to
0.2.
• Start Color: Choose a light blue or white color to resemble snow.
• Emission: Configure the rate at which particles are emitted. Set the rate to create a dense snowfall. For example, you might try starting with 100 particles per second.
• Shape: Choose "Cone" as the shape and adjust the angle and radius to control the area where snowflakes will spawn.
• Gravity Modifier: Apply a downward force (negative value) to simulate gravity. Use a small negative value, like -0.1, to make snowflakes fall gently.

3. Texture for Snowflakes:
• You can use a custom texture for your snowflakes. In the Particle System settings, under the Renderer module, set the Material to a particle material that uses a snowflake texture. Ensure the snowflake texture is set to have a transparent background.
4. Tweak Additional Settings:

• You can further enhance the effect by adjusting settings like Color Over
Lifetime, Size Over Lifetime, and Rotation Over Lifetime to add variation to the snowflakes.
5. Loop the Snowfall (Optional):
• If you want the snowfall to continue indefinitely, check the "Looping" option in the Particle System settings.

6. Play the Scene:
• Press the Play button in Unity to see your snowfall effect in action.
7. Optimization:
• Be mindful of performance. If you have a lot of particles, it can impact your game's performance. Adjust the particle count and other settings as needed for your project.
You can further refine and customize the effect by experimenting with different settings and textures to achieve the desired look for your game or scene.


Practical 8: Develop Android Game with Unity
STEPS:
1. Set Up Unity Project
•	Open Unity Hub and create a new 2D project.
•	Name it "EndlessRunner" and click "Create".
2. Import Required Assets
•	Import sprites for the player character and obstacles. You can find free assets on sites like OpenGameArt or create your own.
3. Create Game Objects
•	In the Hierarchy, create the following:
o	Player: Right-click > 2D Object > Sprite, name it "Player".
o	Obstacle: Right-click > 2D Object > Sprite, name it "Obstacle".
o	Ground: Right-click > 2D Object > Sprite, name it "Ground".
4. Set Up Player
•	Select the Player object.
•	Add a Rigidbody2D component (for physics).
•	Set the Gravity Scale to a small value (e.g., 1) to allow jumping.
•	Add a BoxCollider2D for collision detection.
5. Create Player Movement Script
•	Right-click in the Project window, select Create > C# Script, and name it "PlayerController".
•	Open it and add the following code:
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float jumpForce = 10f;
    private Rigidbody2D rb;

    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    void Update()
    {
        if (Input.GetButtonDown("Jump"))
        {
            rb.velocity = new Vector2(rb.velocity.x, jumpForce);
        }
    }
}
•	Attach this script to the Player object.
6. Set Up Ground
•	Scale the Ground object to act as a floor.
•	Add a BoxCollider2D to it.
7. Create Obstacle Script
•	Create a new script named "ObstacleSpawner".
•	Add the following code to it:
using UnityEngine;

public class ObstacleSpawner : MonoBehaviour
{
    public GameObject obstaclePrefab;
    public float spawnTime = 2f;
    public float spawnY = 0f;

    void Start()
    {
        InvokeRepeating("SpawnObstacle", spawnTime, spawnTime);
    }

    void SpawnObstacle()
    {
        float randomX = Random.Range(1f, 10f);
        Vector2 spawnPosition = new Vector2(randomX, spawnY);
        Instantiate(obstaclePrefab, spawnPosition, Quaternion.identity);
    }
}
8. Set Up Obstacle Prefab
•	Create a prefab from the Obstacle object by dragging it into the Project window.
•	Delete the Obstacle object from the Hierarchy after creating the prefab.
9. Add Obstacle Spawner
•	Create an empty GameObject in the Hierarchy, name it "Spawner", and attach the ObstacleSpawner script to it.
•	Assign the Obstacle prefab to the obstaclePrefab field in the Inspector.
10. Add Collision Detection
•	Update the PlayerController script to detect collisions:
void OnCollisionEnter2D(Collision2D collision)
{
    if (collision.gameObject.CompareTag("Obstacle"))
    {
        // Handle game over (e.g., reload the scene or show a game over screen)
        Debug.Log("Game Over!");
    }
}
•	Make sure to tag the Obstacle prefab as "Obstacle".
11. Build Settings for Android
•	Go to File > Build Settings.
•	Select Android and click "Switch Platform".
•	Make sure to configure the Player settings (e.g., company name, package name).
12. Test and Build
•	Click the Play button in Unity to test the game.
•	Make adjustments as needed.
•	Once satisfied, click "Build" to generate the APK for Android.
Final Touches
•	Add sound effects and music to enhance the gameplay.
•	Implement a scoring system to keep track of the distance traveled.
•	Consider adding menus and game restart functionality.


 

Practical 9: Design and Animate Game Character in Unity.
STEPS:
Step 1: Import Character Model
1.	Download a Model: Get a 3D character from the Unity Asset Store or Mixamo.
2.	Import: In Unity, right-click in the Assets folder and select Import New Asset to add the model.
Step 2: Set Up Character
1.	Create GameObject: Drag the model into the Hierarchy.
2.	Add Components: Add a Rigidbody and a Capsule Collider to the character.
Step 3: Create Animations
1.	Use Mixamo: Upload your character to Mixamo, select animations (Idle, Walk, etc.), and download.
2.	Import Animations: Import these animations into Unity and set the Animation Type to Humanoid.
Step 4: Animator Controller
1.	Create Controller: Right-click in Assets, create Animator Controller, and name it (e.g., CharacterAnimator).
2.	Add States: Drag your animation clips into the Animator window and create transitions.
Step 5: Control with Script
1.	Create Script: Right-click, create C# Script, name it CharacterController.
2.	Add Code:
using UnityEngine;

public class CharacterController : MonoBehaviour
{
    public float moveSpeed = 5f;
    public Animator animator;
    private Rigidbody rb;

    void Start() { rb = GetComponent<Rigidbody>(); }

    void Update()
    {
        float moveHorizontal = Input.GetAxis("Horizontal");
        float moveVertical = Input.GetAxis("Vertical");
        Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
        rb.MovePosition(transform.position + movement * moveSpeed * Time.deltaTime);
        animator.SetBool("isWalking", movement.magnitude > 0);
    }
}
1.	Link Animator: Attach the script to your character and link the Animator component.
Step 6: Set Animation Parameters
1.	Add Parameter: In the Animator, add a Bool parameter called isWalking.
2.	Configure Transitions: Set transitions between Idle and Walk states based on isWalking.
Step 7: Test
•	Play the Game: Press Play and use WASD or arrow keys to move the character and see animations.
Final Touches
•	Add more actions like jumping and sound effects as needed.



 
Practical 10: Create Intelligent enemies in Unity
STEPS:
Step 1: Set Up Enemy Character
1.	Import Model: Import a 3D enemy model into Unity.
2.	Create GameObject: Drag the model into the Hierarchy.
3.	Add Components: Add a Rigidbody and a Capsule Collider.
Step 2: Create Enemy AI Script
1.	Create Script: Right-click in Assets, create C# Script, name it EnemyAI.
2.	Add Code:
using UnityEngine;

public class EnemyAI : MonoBehaviour
{
    public Transform player;
    public float moveSpeed = 3f;
    public float chaseRange = 5f, attackRange = 1.5f;

    void Update()
    {
        float distance = Vector3.Distance(transform.position, player.position);

        if (distance < attackRange) Attack();
        else if (distance < chaseRange) Chase();
        else Idle();
    }

    void Chase() => transform.position = Vector3.MoveTowards(transform.position, player.position, moveSpeed * Time.deltaTime);
    void Attack() => Debug.Log("Attacking the player!");
    void Idle() => Debug.Log("Idle...");
}
Absolutely! Here’s a concise version for creating intelligent enemies in Unity:
Step 1: Set Up Enemy Character
1.	Import Model: Import a 3D enemy model into Unity.
2.	Create GameObject: Drag the model into the Hierarchy.
3.	Add Components: Add a Rigidbody and a Capsule Collider.
Step 2: Create Enemy AI Script
1.	Create Script: Right-click in Assets, create C# Script, name it EnemyAI.
2.	Add Code:
csharp
Copy code
using UnityEngine;

public class EnemyAI : MonoBehaviour
{
    public Transform player;
    public float moveSpeed = 3f;
    public float chaseRange = 5f, attackRange = 1.5f;

    void Update()
    {
        float distance = Vector3.Distance(transform.position, player.position);

        if (distance < attackRange) Attack();
        else if (distance < chaseRange) Chase();
        else Idle();
    }

    void Chase() => transform.position = Vector3.MoveTowards(transform.position, player.position, moveSpeed * Time.deltaTime);
    void Attack() => Debug.Log("Attacking the player!");
    void Idle() => Debug.Log("Idle...");
}
Step 3: Assign Player Target
1.	Create Player Object: Add a player GameObject.
2.	Link Player: Drag the player object to the Player field in the EnemyAI component.
Step 4: Test Your Enemy AI
1.	Press Play: Move the player close to the enemy to see it chase and attack.



 
