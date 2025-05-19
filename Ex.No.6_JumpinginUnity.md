# Experiment 6: Implementing Jumping Behavior in Unity 
### DATE:   25-03-2025
### REGISTER NUMBER : 212222243003

### AIM: 
To implement a jumping behavior for a player-controlled character in Unity using Rigidbody physics and user input, where pressing the spacebar while in the air causes the player to move down. 
### Algorithm:
```
1. **Create a New Unity Project**  
   - Open **Unity Hub** → Click **New Project**  
   - Select **3D Template** → Name the project **JumpingBehavior** → Click **Create**  

2. **Set Up the Scene**  
   - **Create Ground:**  
     - Go to **GameObject → 3D Object → Plane**  
     - Rename it `"Ground"`  
     - Scale it to **(10,1,10)** (or as needed)  
     - Set **Position** to `(0, 0, 0)`  

3. **Create the Player Object**  
   - **Create a Capsule to Represent the Player:**  
     - Go to **GameObject → 3D Object → Capsule**  
     - Rename it `"Player"`  
     - Scale it to **(1,2,1)** (default is fine)  
     - Set **Position** to `(0, 1, 0)`  

4. **Add Rigidbody to the Player**  
   - Select **Player**  
   - Go to **Inspector → Add Component → Rigidbody**  
   - Set the **Constraints**:  
     - Freeze **Rotation X**, **Rotation Z** (to prevent falling over)  

5. **Create the Jumping Script**  
   - Go to **Assets → Right Click → Create → C# Script**  
   - Rename it `"Jump.cs"`  
   - Open the script and add the following code:  

6. **Attach the Script to the Player**  
   - Select **Player**  
   - Drag & Drop **Jump.cs** onto it  

7. **Assign the Ground Tag**  
   - Select **Ground**  
   - In **Inspector**, go to **Tag → Add Tag... → Click "+" → Type "Ground" → Save**  
   - Select **Ground** again and assign the `"Ground"` tag  

8. **Run the Program**  
   - Press **Play**  
   - Press **Spacebar** to make the player jump, and if pressed while in the air, the player will fall down  
```  
### Program:
```
using UnityEngine;

   public class Jump : MonoBehaviour
   {
       public float jumpForce = 5f;
       private bool isGrounded;
       private Rigidbody rb;

       void Start()
       {
           rb = GetComponent<Rigidbody>();
       }

       void Update()
       {
           if (isGrounded && Input.GetKeyDown(KeyCode.Space))
           {
               rb.AddForce(Vector3.up * jumpForce, ForceMode.Impulse);
               isGrounded = false;
           }
       }

       private void OnCollisionEnter(Collision collision)
       {
           if (collision.gameObject.CompareTag("Ground"))
           {
               isGrounded = true;
           }
       }
   }
```

### Output:

## Initially player in air:
<img width="850" alt="image" src="https://github.com/user-attachments/assets/7a669489-0f94-4e03-a42c-16db1298e719" />

## After player jumped:
<img width="850" alt="image" src="https://github.com/user-attachments/assets/3c43b7e2-e1c4-4e34-822c-ad2aff291b84" />








### Result:
Thus the simple seek behavior was implemented successfully.
