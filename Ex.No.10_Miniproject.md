# Ex.No: 10  Implementation of Jumping game
### Name : paida ram sai
### REGISTER NUMBER : 212223110034
### AIM: 
To develop a game on control player movement and jump in 2D using Rigidbody2D.
### Algorithm:
```
PlayerController.cs

1.Initialize Rigidbody2D component in Start.
2.Every frame (Update):
   a. Read horizontal input value.
   b. Apply movement by updating the Rigidbody2D velocity.
   c. If space key is pressed and the player is on the ground, apply jump force.
3.Detect ground contact using OnCollisionEnter2D to set isGrounded true.
4.Detect leaving the ground using OnCollisionExit2D to set isGrounded false.
```
```
Collectible.cs

1.Trigger detection using OnTriggerEnter2D.
2.Check if the object entering the trigger is tagged "Player".
3.If true, award points using ScoreManager.instance.AddScore().
4.Delete collectible from the scene using Destroy().
5.Provides scoring mechanics through collectible items.
```
```
Enemy.cs

1.Detect collisions through OnCollisionEnter2D.
2.If the collided object has the "Player" tag:
   a. Restart the level by loading scene 0 (active scene).
3.This simulates "player death" when touching an enemy.
4.Ensures challenge by resetting game progress on collision.
```
```
GameManager.cs

1.Create global singleton instance in Awake for easy game control access.
2.RestartGame() function reloads the currently active level using SceneManager.
3.Allows other scripts to trigger a reset (e.g., enemies, traps).
4.Ensures consistent game flow and centralized game management.
```
```
Parallax.cs

1.Get camera transform in Start for tracking camera movement.
2.Store initial camera position to detect movement delta.
3.Every frame (Update):
  a. Compute difference between current and previous camera positions.
  b. Move background layer based on delta multiplied by parallaxSpeed.
  c. Update previous camera position to the new one.
4.Creates depth illusion by making backgrounds move slower than camera.
```  
### Program:
PlayerController.cs
```
using UnityEngine;

public class PlayerController : MonoBehaviour
{
public float moveSpeed = 5f;
public float jumpForce = 7f;
bool isGrounded;

Rigidbody2D rb;

void Start()
{
    rb = GetComponent<Rigidbody2D>();
}

void Update()
{
    float move = Input.GetAxis("Horizontal");
    rb.velocity = new Vector2(move * moveSpeed, rb.velocity.y);

    if (Input.GetKeyDown(KeyCode.Space) && isGrounded)
    {
        rb.velocity = new Vector2(rb.velocity.x, jumpForce);
    }
}

private void OnCollisionEnter2D(Collision2D collision)
{
    if (collision.collider.CompareTag("Ground"))
        isGrounded = true;
}

private void OnCollisionExit2D(Collision2D collision)
{
    if (collision.collider.CompareTag("Ground"))
        isGrounded = false;
}


}
```
Collectible.cs
```
using UnityEngine;

public class Collectible : MonoBehaviour
{
void OnTriggerEnter2D(Collider2D other)
{
if (other.CompareTag("Player"))
{
ScoreManager.instance.AddScore(10);
Destroy(gameObject);
}
}
}
```
Enemy.cs
```
using UnityEngine;

public class Enemy : MonoBehaviour
{
void OnCollisionEnter2D(Collision2D collision)
{
if (collision.collider.CompareTag("Player"))
{
UnityEngine.SceneManagement.SceneManager.LoadScene(0);
}
}
}

```
GameManager.cs
```
using UnityEngine;
using UnityEngine.SceneManagement;

public class GameManager : MonoBehaviour
{
public static GameManager instance;

void Awake()
{
    instance = this;
}

public void RestartGame()
{
    SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex);
}
}
```
Parallax.cs

```
using UnityEngine;

public class Parallax : MonoBehaviour
{
public float parallaxSpeed = 0.5f;
Transform cam;
Vector3 previousCamPos;

void Start()
{
    cam = Camera.main.transform;
    previousCamPos = cam.position;
}

void Update()
{
    Vector3 delta = cam.position - previousCamPos;
    transform.position += new Vector3(delta.x * parallaxSpeed, delta.y * parallaxSpeed, 0);
    previousCamPos = cam.position;
}


}
```
### Output:
<img width="1471" height="754" alt="image" src="https://github.com/user-attachments/assets/1732bcec-503f-47f9-8242-357dcb521d9b" />
<img width="1478" height="761" alt="image" src="https://github.com/user-attachments/assets/c6acff2b-a6f1-4d6f-b90c-f136c4f3c401" />
<img width="1474" height="756" alt="image" src="https://github.com/user-attachments/assets/da620950-3b76-4f86-8e62-696e870672b7" />



### Result:
Thus the game was developed using Unity and it is successfully executed.
