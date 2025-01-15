
---
```
ResetBuffer(bool force)
```


This method resets a buffer which as I understand is a representation of a local map where we accumulate point clouds. Therefore, when we reset a buffer we need to also provide the reference frame of the local map that will be represented by the resetted buffer.  

---
```
bool AddPointCloudToBuffer(
	pcl::PointCloud<pcl::PointXYZ>::Ptr& point_cloud,
	Eigen::Matrix3f& rotation_matrix, 
	Eigen::Vector3f& translation_vector
)
```


