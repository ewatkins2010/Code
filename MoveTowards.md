using UnityEngine;
using System.Collections;


public class MoveTowards : MonoBehaviour {
	
	
	private Vector2 target;
	public GameObject followed;
	public GameObject[] enemies;
	public GameObject p;
	public int moveSpeed;
	float distance;
	float closest = 200;
	
	
	void Start(){
		enemies = GameObject.FindGameObjectsWithTag ("Enemy");
		p = GameObject.FindGameObjectWithTag ("Player");
		foreach (GameObject e in enemies) {
			distance = Vector2.Distance (e.transform.position, transform.position);
			
			if (distance < closest && e.renderer.isVisible) {
				closest = distance;
				followed = e;
			}
		}
	}

	void Update () 
	{
		if (followed != null) {
			target = followed.transform.position;
			Vector3 dir = followed.transform.position - transform.position;
			float angle = Mathf.Atan2 (dir.y, dir.x) * Mathf.Rad2Deg;
			if (p.GetComponent<PlayerController> ().isRight == 1)
				transform.rotation = Quaternion.AngleAxis (angle, Vector3.forward);
			else
				transform.rotation = Quaternion.AngleAxis (angle - 180, Vector3.forward);
			transform.position = Vector2.MoveTowards (transform.position, target, moveSpeed * Time.deltaTime);
		} 
		else {
			this.rigidbody2D.AddForce (transform.forward * moveSpeed);
		}
	}
}

