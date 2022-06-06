##  Lidar 3D point cloud labeling with Velodyne Lidar sensor in SageMaker GroundTruth

Associated blog: (TBD)

This notebook will demonstrate how to pre-process LiDAR sensor data to create an object tracking labeling job with Sensor fusion in SageMaker Ground Truth.

For object tracking, you will track the movement of an object (e.g., car, pedestrian) while your point of reference (in this case, the car) is moving. You will experiment with coverting your 3D point cloud data from local coordinates to the world coordinate system to keep everything in the frame of reference.

We will also include camera image leveraging the sensor fusion feature in SageMaker Ground Turth to provide labeling workers more visual information about the scene they are labeling. Through sensor fusion, workers will be able to adjust labels in the 3D scene and in 2D images, and label adjustments will be mirrored in the other view.

The dataset used is provided to us by Velodyne. We will go over the dataset content in detail in later sections.

## Prerequisites

- An S3 bucket you can write to. The bucket must be in the same region as this SageMaker Notebook instance. You can also define a valid S3 prefix. All the files related to this experiment will be stored in that prefix of your bucket. ***Important: you must attach the CORS policy to this bucket.** To learn how to add a CORS policy to your S3 bucket, follow the instructions in [How do I add cross-domain resource sharing with CORS?](https://docs.aws.amazon.com/AmazonS3/latest/userguide/enabling-cors-examples.html). Paste the following policy in the CORS configuration editor:

```
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<CORSRule>
    <AllowedOrigin>*</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedMethod>HEAD</AllowedMethod>
    <AllowedMethod>PUT</AllowedMethod>
    <MaxAgeSeconds>3000</MaxAgeSeconds>
    <ExposeHeader>Access-Control-Allow-Origin</ExposeHeader>
    <AllowedHeader>*</AllowedHeader>
</CORSRule>
<CORSRule>
    <AllowedOrigin>*</AllowedOrigin>
    <AllowedMethod>GET</AllowedMethod>
</CORSRule>
</CORSConfiguration>
```

- Download pcd.py (run the code block below): pyntcloud is a Python 3 library for working with 3D point clouds. This module allows us to work with the .pcd files generated from the LiDAR sensors
- Familiarity with the [Ground Truth 3D Point Cloud Labeling Job](https://docs.aws.amazon.com/sagemaker/latest/dg/sms-point-cloud.html).
- Familiarity with Python and numpy.
- Basic understanding of [AWS Sagemaker](https://aws.amazon.com/sagemaker/).
- Basic familiarity with [AWS Command Line Interface (CLI)](https://aws.amazon.com/cli/)

This notebook has only been tested on a SageMaker notebook instance. We used an ml.t3.medium instance in our tests.


## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

