
# Ex.No: 8  Implementation of Path finding using A* algorithm
### DATE:  07/04/25                                                                          
### REGISTER NUMBER :  212223240112
### AIM: 
To write a program to create graph using waypoints and use A* algorithm to find path between source and destination.
### Algorithm:

1. Create a New Unity Project by Open the  Unity Hub and create a new 3D Project,Name the project (e.g., Pathfinding).


2. Create Waypoints in Scene => Create empty or sphere GameObjects ( minimum 4)  and  name it as Waypoint1, Waypoint2, ..., Waypoint4
   Position them freely in the scene (not on a grid)


   
4. Write a waypoint script to draw the lines between neighbors.

5. Attach waypoint.cs script to each waypoint GameObject and Manually assign neighbors in the Inspector to form a graph.


6. Create a empty game object and name it as WaypointManager - to manage all waypoints


7. Attach Waypoint script to it

8.Write a Pathfinding algorithm using A*search

9. Create a Game Object for Player ( choose capsule or any others) and attach the script to move player from start to end waypoints
 
### Program:
```
**#1.Waypoint.cs**
using System.Collections.Generic;
using UnityEngine;

public class Waypoint : MonoBehaviour
{
    public List<Waypoint> neighbors = new List<Waypoint>();

    private void OnDrawGizmos()
    {
        Gizmos.color = Color.red;
        foreach (Waypoint neighbor in neighbors)
        {
            if (neighbor != null)
                Gizmos.DrawLine(transform.position, neighbor.transform.position);
        }
    }
}
**#2. Waypointmanager.cs**
using System.Collections.Generic;
using UnityEngine;

public class WaypointManager : MonoBehaviour
{
    public List<Waypoint> waypoints = new List<Waypoint>();

    public List<Waypoint> FindPath(Waypoint start, Waypoint end)
    {
        List<Waypoint> openSet = new List<Waypoint> { start };
        HashSet<Waypoint> closedSet = new HashSet<Waypoint>();
        Dictionary<Waypoint, Waypoint> cameFrom = new Dictionary<Waypoint, Waypoint>();

        Dictionary<Waypoint, float> gScore = new Dictionary<Waypoint, float>();
        Dictionary<Waypoint, float> fScore = new Dictionary<Waypoint, float>();

        foreach (Waypoint wp in waypoints)
        {
            gScore[wp] = float.MaxValue;
            fScore[wp] = float.MaxValue;
        }

        gScore[start] = 0;
        fScore[start] = Vector3.Distance(start.transform.position, end.transform.position);

        while (openSet.Count > 0)
        {
            Waypoint current = openSet[0];
            foreach (Waypoint w in openSet)
                if (fScore[w] < fScore[current]) current = w;

            if (current == end)
            {
                List<Waypoint> path = new List<Waypoint>();
                while (cameFrom.ContainsKey(current))
                {
                    path.Add(current);
                    current = cameFrom[current];
                }
                path.Add(start);
                path.Reverse();
                return path;
            }

            openSet.Remove(current);
            closedSet.Add(current);

            foreach (Waypoint neighbor in current.neighbors)
            {
                if (closedSet.Contains(neighbor)) continue;

                float tentativeG = gScore[current] + Vector3.Distance(current.transform.position, neighbor.transform.position);
                if (!openSet.Contains(neighbor)) openSet.Add(neighbor);
                else if (tentativeG >= gScore[neighbor]) continue;

                cameFrom[neighbor] = current;
                gScore[neighbor] = tentativeG;
                fScore[neighbor] = gScore[neighbor] + Vector3.Distance(neighbor.transform.position, end.transform.position);
            }
        }

        return null;
    }
}

**#3.playermover.cs**
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerMover : MonoBehaviour
{
    public Waypoint start;
    public Waypoint end;
    public WaypointManager manager;
    public float speed = 3f;
    private List<Waypoint> path;
    private int index = 0;

    void Start()
    {
        path = manager.FindPath(start, end);
        if (path != null && path.Count > 0)
            StartCoroutine(FollowPath());
    }

    IEnumerator FollowPath()
    {
        while (index < path.Count)
        {
            Vector3 target = path[index].transform.position;
            while (Vector3.Distance(transform.position, target) > 0.1f)
            {
                transform.position = Vector3.MoveTowards(transform.position, target, speed * Time.deltaTime);
                yield return null;
            }
            index++;
            yield return null;
        }
    }
}
```

Check the following
1. Waypoints placed in scene
2. Neighbors set manually via Inspector
3. WaypointGraph script on a manager
4. AICharacter assigned a start and goal

## Output:
### Initial player position:
<img width="850" alt="image" src="https://github.com/user-attachments/assets/bbc659bd-74dd-4d4a-9a00-a485f621b193" />


### After pathfinding player position:
<img width="850" alt="image" src="https://github.com/user-attachments/assets/d13173ad-ade8-45d8-af15-0f14feb81608" />









### Result:
Thus the pathfinding algorithm was sucessfully implemented.
