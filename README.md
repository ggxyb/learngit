#UNITY-space shooter
2018/11/3 19:05:20 
实在有点忙，感觉完不成了。本来想选AI，但基础差，慌乱中选择了unity游戏，从上午一直做到晚上，放弃了吃饭，但还是没完成，也许教程版本过旧，但一个入门级的任务都没有完成，我实在对不起所有人，或许我该平衡一下学习时间和任务时间。
#1   
## 
###  
###1）安好了unity平台，没有遇到大问题。
###2）导入时遇到导断网，加入收藏后导入成功……
###3）player setting遇到教程中未出现的问题：界面分辨率不能自动调整，只能暂时用其他方法代替，可能由于发布平台下载时出错（实际因教程版本过旧，网上查过，但没有解决办法）。
#2添加飞机、火焰、comllider
## 
###1）从素材中拖入即可。
###2）注意飞机重力问题。
#3设置摄像机与灯光
###1）选择主camera，rotation-x-90，position-y-10；z-15。
###2）因教程版本过旧没有找到rendening-setting。
####1*main light：x-20 y-（-115）
####2*fill light：x-5 y-115
####3*rim light：x-（-20—）y-65
#4设置背景
##  
###1）creat->3D object->quad
###2）设置背景大小scale：x-15 y-30，亮度：shader-texture
###3）背景下移，与飞船分开
#5控制飞船移动
## 
creat一个脚本，添加到player中。
	上代码：
	[System.Serializable]
	public class Boundary
	{
	    public float xMin, xMax, zMin, zMax;
	}
	
		public class playercontroller : MonoBehaviour
	{
	    public float speed = 10f;   //飞船速度
	    public Boundary boundary;   //持有Boundary，，不懂
	    public float tilt = 4f;
	    private void FixedUpdate()
	    {
	        //获取键盘输入
	        float moveHorizontal = Input.GetAxis("Horizontal");
	        float moveVertical = Input.GetAxis("Vertical");



        //1.控制移动
        Vector3 movement = new Vector3(moveHorizontal, 0f, moveVertical);
        this.GetComponent<Rigidbody>().velocity = movement* speed;


        //限制位置
        this.GetComponent<Rigidbody>().position = new Vector3
            (Mathf.Clamp(this.GetComponent<Rigidbody>().position.x, boundary.xMin, boundary.xMax),
                0f,
                Mathf.Clamp(this.GetComponent<Rigidbody>().position.z, boundary.zMin, boundary.zMax)
            );

        //倾斜效果
        this.GetComponent<Rigidbody>().rotation = Quaternion.Euler(0f, 0f, this.GetComponent<Rigidbody>().velocity.x * -tilt);
    }
}
#6创建子弹，实现视觉和逻辑功能
## 
	public class mover : MonoBehaviour
	{
	    public float speed = 20f;
	    private void Start()
	    {
	        this.GetComponent<Rigidbody>().velocity = this.transform.forward * speed;
	    }
	
	}
