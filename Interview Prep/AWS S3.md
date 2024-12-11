#### Amazon S3 Basics:
  - **Data Storage**: Stores data as objects within buckets.
  - **Objects**: A file and any metadata describing the file.
  - **Buckets**: Containers for objects.
  - **Creating Buckets**: Specify a bucket name and AWS Region.

- **Uploading Data**:
  - Data is uploaded to buckets as objects.
  - Each object has a key (key name) which is a unique identifier within the bucket.

- **Features**:
  - **S3 Versioning**: Keeps multiple versions of an object in the same bucket.
    - Allows restoration of objects that are accidentally deleted or overwritten.

- **Access Control**:
  - Buckets and objects are private by default.
  - Access is managed through explicitly granted permissions.
  - **Access Management Tools**:
    - **Bucket Policies**: Define access rules for the entire bucket.
    - **IAM Policies**: Manage permissions at the user or group level.
    - **S3 Access Points**: Simplify managing access to shared data sets.
    - **Access Control Lists (ACLs)**: Fine-grained control over individual objects and buckets.

### Multipart Upload
Multipart upload allows you to upload a single object as a set of parts. Each part is a contiguous portion of the object's data. You can upload these object parts independently and in any order. If transmission of any part fails, you can retransmit that part without affecting other parts. After all parts of your object are uploaded, Amazon S3 assembles these parts and creates the object. In general, when your object size reaches 100 MB, you should consider using multipart uploads instead of uploading the object in a single operation.

Using multipart upload provides the following advantages:

- **Improved throughput** – You can upload parts in parallel to improve throughput.
    
- **Quick recovery from any network issues** – Smaller part size minimizes the impact of restarting a failed upload due to a network error.
    
- **Pause and resume object uploads** – You can upload object parts over time. After you initiate a multipart upload, there is no expiry; you must explicitly complete or stop the multipart upload.
    
- **Begin an upload before you know the final object size** – You can upload an object as you are creating it.