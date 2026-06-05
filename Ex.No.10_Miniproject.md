# Ex.No: 10  Implementation of 2D/3D game using AI-based navigation techniques.
### DATE: 05/06/2026                                                                        
### REGISTER NUMBER : 212224220044
### AIM: 
To develop a tank navigation game in Unity using AI pathfinding.

### Algorithm:
1.Open Unity and create a new 3D project.

2.Import the required terrain, environment, and vehicle assets.

3.Design the desert battlefield with obstacles, buildings, rocks, and landmarks.

4.Add a tank model and configure its components.

5.Create waypoints and destination points such as Ruin, Rock, Helicopter, and Factory.

6.Implement AI navigation using Unity's NavMesh system.

7.Bake the NavMesh to enable pathfinding across the terrain.

8.Develop scripts to control tank movement toward selected destinations.

9.Create a UI panel with buttons for different target locations.

10.Link each button to the corresponding destination using C# scripts.

11.Test the game and verify that the tank navigates correctly while avoiding obstacles.

12.Build and run the project.

### Program:
## FollowWayPoints
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class FollowWaypoints : MonoBehaviour {

    Transform goal;
    float speed = 5.0f;
    float accuracy = 5.0f;
    float rotSpeed = 2.0f;
    GameObject[] wps;
    GameObject currentNode;
    int currentWP = 0;
    Graph g;

    public GameObject wpManager;

    void Start() {
        Time.timeScale = 5.0f;
        wps = wpManager.GetComponent<WPManager>().waypoints;
        g = wpManager.GetComponent<WPManager>().graph;
        currentNode = wps[0];

        // Invoke("GotoRuin", 2.0f);
    }

    public void GotoHeli() {

        g.AStar(currentNode, wps[0]);
        currentWP = 0;
    }

    public void GotoRuin() {

        g.AStar(currentNode, wps[7]);
        currentWP = 0;
    }

    public void GotoRock() {

        g.AStar(currentNode, wps[1]);
        currentWP = 0;
    }

    public void GotoFactory() {

        g.AStar(currentNode, wps[4]);
        currentWP = 0;
    }

    void LateUpdate() {

        if (g.pathList.Count == 0 || currentWP == g.pathList.Count) return;

        currentNode = g.getPathPoint(currentWP);

        if (Vector3.Distance(g.pathList[currentWP].getID().transform.position, transform.position) < accuracy) {

            currentWP++;
        }

        if (currentWP < g.pathList.Count) {

            goal = g.pathList[currentWP].getID().transform;
            Vector3 lookAtGoal = new Vector3(
                goal.position.x,
                transform.position.y,
                goal.position.z);

            Vector3 direction = lookAtGoal - this.transform.position;

            transform.rotation = Quaternion.Slerp(
                this.transform.rotation,
                Quaternion.LookRotation(direction),
                Time.deltaTime * rotSpeed);

            transform.Translate(0.0f, 0.0f, speed * Time.deltaTime);
        }
    }
}
```
## WayPointDebug
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[ExecuteInEditMode]
public class WaypointDebug : MonoBehaviour {

	void RenameWPs(GameObject overlook)
	{
		GameObject[] gos;
	    gos = GameObject.FindGameObjectsWithTag("wp"); 
	    int i = 1;
	    foreach (GameObject go in gos)  
	    { 
	     	if(go != overlook)
	     	{
	     		go.name = "WP" + string.Format("{0:000}",i); 
	     		i++; 
	     	} 
	    }	
	}

	void OnDestroy()
	{
		RenameWPs(this.gameObject);
	}

	// Use this for initialization
	void Start () {
		if(this.transform.parent.gameObject.name != "WayPoint") return;
		RenameWPs(null);
	}
	
	// Update is called once per frame
	void Update () {
		this.GetComponent<TextMesh>().text = this.transform.parent.gameObject.name;
	}
}
```
## WPmanager
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public struct Link {

    public enum direction {

        UNI,
        BI
    }

    public GameObject node1, node2;
    public direction dir;
}

public class WPManager : MonoBehaviour {

    public GameObject[] waypoints;
    public Link[] links;
    public Graph graph = new Graph();

    void Start() {


        if (waypoints.Length > 0) {

            foreach (GameObject wp in waypoints) {

                graph.AddNode(wp);

                foreach (Link l in links) {

                    graph.AddEdge(l.node1, l.node2);
                    if (l.dir == Link.direction.BI) {

                        graph.AddEdge(l.node2, l.node1);
                    }
                }
            }
        }
    }
}
```
### Output:

<img width="1919" height="1135" alt="Screenshot 2026-06-04 131928" src="https://github.com/user-attachments/assets/3599e944-b29d-4ad8-85be-b20c50d00079" />


### Result:
Thus, the game was developed using Unity and adopted NavMesh-based AI technology.
