---
title: Eigen中四元数、欧拉角、旋转向量、旋转矩阵的转换
categories: ProjectExperience
---

## 四元数

```
1.0 初始化四元数
Eigen::Quaterniond quaternion(w,x,y,z); //初始化

1.1 四元数转旋转向量
（1）Eigen::AngleAxisd rotation_vector(quaternion);
（2）Eigen::AngleAxisd rotation_vector;      rotation_vector=quaternion;

1.2 四元数转旋转矩阵
（1）Eigen::bool astar::changeCurrentPtAtt(astar_node_ptr& PointPtr, const Polyhedron3D& poly) {
    vec_E<std::pair<Vec3f, Vec3f>> pt_norm_pair = poly.cal_normals();

    double max_distance = 0.0, min_distance = 4 * voxbloxMapPtr_->voxelSize_;
    point_3d dir_max, dir_min;
    point_3d pt_max, pt_min;Matrix3d rotation_matrix;	rotation_matrix= quaternion.matrix();
（2）Eigen::Matrix3d rotation_matrix;	rotation_matrix=quaternion.toRotationMatrix();

1.3 四元数转欧拉角(Z-Y-X，即RPY)
Eigen::Vector3d eulerAngle=quaternion.matrix().eulerAngles(2,1,0);
```



## 欧拉角

```
2.0 初始化欧拉角(Z-Y-X，即YPR)
Eigen::Vector3d eulerAngle(yaw,pitch,roll);

2.1 欧拉角转旋转向量
Eigen::AngleAxisd rollAngle(AngleAxisd(eulerAngle(2),Vector3d::UnitX()));
Eigen::AngleAxisd pitchAngle(AngleAxisd(eulerAngle(1),Vector3d::UnitY()));
Eigen::AngleAxisd yawAngle(AngleAxisd(eulerAngle(0),Vector3d::UnitZ())); 
Eigen::AngleAxisd rotation_vector;
rotation_vector=yawAngle*pitchAngle*rollAngle;

2.2 欧拉角转旋转矩阵
Eigen::AngleAxisd rollAngle(Eigen::AngleAxisd(eulerAngle(2),Eigen::Vector3d::UnitX()));
Eigen::AngleAxisd pitchAngle(Eigen::AngleAxisd(eulerAngle(1),Eigen::Vector3d::UnitY()));
Eigen::AngleAxisd yawAngle(Eigen::AngleAxisd(eulerAngle(0),Eigen::Vector3d::UnitZ())); 
Eigen::Matrix3d rotation_matrix;
rotation_matrix=yawAngle*pitchAngle*rollAngle;
2.3 欧拉角转四元数
Eigen::AngleAxisd rollAngle(AngleAxisd(eulerAngle(2),Vector3d::UnitX()));
Eigen::AngleAxisd pitchAngle(AngleAxisd(eulerAngle(1),Vector3d::UnitY()));
Eigen::AngleAxisd yawAngle(AngleAxisd(eulerAngle(0),Vector3d::UnitZ())); Eigen::Quaterniond quaternion;
quaternion=yawAngle*pitchAngle*rollAngle;
```





## 旋转向量

```
1.0 初始化旋转向量：旋转角为alpha，旋转轴为(x,y,z)
Eigen::AngleAxisd rotation_vector(alpha,Vector3d(x,y,z))

1.1 旋转向量转旋转矩阵
Eigen::Matrix3d rotation_matrix;rotation_matrix=rotation_vector.matrix();
Eigen::Matrix3d rotation_matrix;rotation_matrix=rotation_vector.toRotationMatrix();

1.2 旋转向量转欧拉角(Z-Y-X，即RPY)
Eigen::Vector3d eulerAngle=rotation_vector.matrix().eulerAngles(2,1,0);

1.3 旋转向量转四元数
Eigen::Quaterniond quaternion(rotation_vector);
Eigen::Quaterniond quaternion;Quaterniond quaternion;
Eigen::Quaterniond quaternion;quaternion=rotation_vector;
```



## 旋转矩阵

```
2.0 初始化旋转矩阵
Eigen::Matrix3d rotation_matrix;rotation_matrix<<x_00,x_01,x_02,x_10,x_11,x_12,x_20,x_21,x_22;

2.1 旋转矩阵转旋转向量
Eigen::AngleAxisd rotation_vector(rotation_matrix);
Eigen::AngleAxisd rotation_vector;rotation_vector=rotation_matrix;
Eigen::AngleAxisd rotation_vector;rotation_vector.fromRotationMatrix(rotation_matrix);

2.2 旋转矩阵转欧拉角(Z-Y-X，即RPY)
Eigen::Vector3d eulerAngle=rotation_matrix.eulerAngles(2,1,0);

2.3 旋转矩阵转四元数
Eigen::Quaterniond quaternion(rotation_matrix);
Eigen::Quaterniond quaternion;
quaternion=rotation_matrix;
```

旋转矩阵与姿态的关系：
$$
M_B^W = [X_B^W \ Y_B^W \ Z_B^W] \\
M_B^W表示Body到Wolrd的坐标系 \\
X_B^W表示Body系X轴在Wolrd系的投影
$$