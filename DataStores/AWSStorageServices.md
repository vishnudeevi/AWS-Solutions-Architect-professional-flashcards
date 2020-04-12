### AWS Storage Services

* ##### AWS Storage services white paper - https://d1.awsstatic.com/whitepapers/Storage/AWS%20Storage%20Services%20Whitepaper-v9.pdf

* ##### AWS S3 FAQ - https://aws.amazon.com/s3/faqs/

#### Types of Storage solutions provided by Amazon
```
A. Block Storage

B. File Storage

C. Object Strage

D. None
```
<details><summary>show</summary>
<p>ABC</p>
</details>


#### S3 - Describe storage types provides and their categories
|Storage service   |Description   |Storage type|
|---|---|--|
| Amazon Simple Storage Service(Amazon S3)|A service that provides scalable and highlydurable object storage in the cloud.| Object storage|
|   |   | |
|   |   | |
|   |   | |
<details><summary>show</summary>
<p>

</p>
</details>

#### S3 - Storage Classes - Amazon S3 offers a range of storage classes designed for different use cases (https://aws.amazon.com/s3/storage-classes/)
<details><summary>show</summary>
<p>

```
A. Amazon S3 Standard, for general-purpose storage of frequently accessed data

B. Amazon S3 Standard-Infrequent Access (Standard-IA), for long-lived, but less frequently accessed data

C. S3 Intelligent-Tiering for data with unknown or changing access patterns; 

D. S3 Standard-Infrequent Access (S3 Standard-IA) and S3 One Zone-Infrequent Access (S3 One Zone-IA) for long-lived, but less frequently accessed data; 

E. Amazon S3 Glacier (S3 Glacier) and Amazon S3 Glacier Deep Archive (S3 Glacier Deep Archive) for long-term archive and digital preservation.

```
</p>
</details>


#### S3 - Usgage - What are Amazon S3 four common usage patterns 
<details><summary>show</summary>
<p>

```
    
1. Amazon S3 can be used to serve static content particularly for web its easy as it provides http access for all objects and it could serve heavy traffics and also this content could be exposed through CDN.

2. Amazon S3 can host entire websites from html, media, client side scripts like java script. 
    
3.  Amazon S3 is used as a data store for computation and large-scale analytics, such as financial transaction analysis, clickstream analytics, and media transcoding

4.  Amazon S3 is often used as a highly durable, scalable, and secure solution for backup and archiving of critical data. You can easily move cold data to Amazon Glacier using lifecycle management rules on data stored in Amazon
S3. You can also use Amazon S3 cross-region replication to automatically copy objects across S3 buckets in different AWS Regions asynchronously, providing disaster recovery solutions for business continuity.
```
</p>
</details>

#### S3 - Limitations - What are Amazon S3 limitations interms of no of objects, object size and upload size
<details><summary>show</summary>
<p>

```
1. The total volume of data and number of objects you can store are unlimited. 

2. Individual Amazon S3 objects can range in size from a minimum of 0 bytes to a maximum of 5 terabytes. 

3. The largest object that can be uploaded in a single PUT is 5 gigabytes. 

4. For objects larger than 100 megabytes, customers should consider using the Multipart Upload capability. 
```
</p>
</details>



#### S3 - Performance - How to improve performace on S3 interms of object access and upload 
<details><summary>show</summary>
<p>

```
1. If AWS S3 objects are accessed from an EC2 instance keep them in same region
    
2. If S3 objects are more than 100MB upload those objects through multipart upload API.

3. Use Cloud Search /RDS or DynamoDB to store S3 object metadata (name, size, keywords) to quickly search objects and store actual objects in S3. 

4. Amazon S3 Transfer Acceleration enables fast, easy, and secure transfer of files over long distances between your client and your Amazon S3 bucket. It leverages Amazon CloudFront globally distributed edge locations to route traffic to your Amazon S3 bucket over an Amazon-optimized network path
```
</p>
</details>


#### What is a Provisioned Capacity Unit (PCU) and when should it use PCU?

<details><summary>show</summary>
<p>

```
Provisioned Capacity guarantees that your retrieval capacity for Expedited retrievals will be available when you need it. Each unit of capacity ensures that at least 3 expedited retrievals can be performed every 5 minutes and provides up to 150MB/s of retrieval throughput. Retrieval capacity can be provisioned if you have specific Expedited retrieval rate requirements that need to be met. Without provisioned capacity, Expedited retrieval requests will be accepted if capacity is available at the time the request is made. You can purchase provisioned capacity using the console, SDK, or the CLI. Each unit of provisioned capacity costs $100 per month from the date of purchase.
    
```
</p>
</details>

#### S3 - Billing - How is S3 usage charges 

<details><summary>show</summary>
<p>
    
```
There is no Data Transfer charge for data transferred within an Amazon S3 Region via a COPY request. 

Data transferred via a COPY request between AWS Regions is charged at rates specified in the pricing section of the Amazon S3 detail page. 

There is no Data Transfer charge for data transferred between Amazon EC2 and Amazon S3 within the same region, for example, data transferred within the US East (Northern Virginia) Region. 

However, data transferred between Amazon EC2 and Amazon S3 across all other regions is charged at rates specified on the Amazon S3 pricing page, for example, data transferred between Amazon EC2 US East (Northern Virginia) and Amazon S3 US West (Northern California).
```
</p>
</details>

#### S3 - Billing - How is billing done in terms of S3 buckets ? (https://aws.amazon.com/s3/pricing/)

<details><summary>show</summary>
<p>

```
A. Storage used by buckets (Objects and metadata).

B.  Network data transferred IN.
    
C.  Network data transferred OUT.

D. Data Requests.
    
E. Data Retrieval.
    
```
</p>
</details>

#### S3 - Security - Access to data stored in S3 ?

<details><summary>show</summary>
<p>

```
    
A.IAM enables organizations with multiple employees to create and manage multiple users under a single AWS account. With IAM policies, customers can grant IAM users fine-grained control to their Amazon S3 bucket or objects while also retaining full control over everything the users do.

B. With bucket policies, customers can define rules which apply broadly across all requests to their Amazon S3 resources, such as granting write privileges to a subset of Amazon S3 resources. Customers can also restrict access based on an aspect of the request, such as HTTP referrer and IP address.
    
C. With ACLs, customers can grant specific permissions (i.e. READ, WRITE, FULL_CONTROL) to specific users for an individual bucket or object.
    
D.With Query String Authentication, customers can create a URL to an Amazon S3 object which is only valid for a limited time. 
    
```
</p>
</details>
    
#### S3 - Durability - How is S3 Durability ?
<details><summary>show</summary>
<p>

```
Amazon S3 Standard, S3 Standard-Infrequent Access, and S3 Glacier storage classes replicate data across a minimum of three AZs to protect against the loss of one entire AZ. This remains true in Regions where fewer than three AZs are publicly available. Objects stored in these storage classes are available for access from all of the AZs in an AWS Region.

The Amazon S3 One Zone-IA storage class replicates data within a single AZ. Data stored in this storage class is susceptible to loss in an AZ destruction event.

|Storage Type|Durability|Availability|
|---|---|---|
|S3 Standard|99.999999999%|99.99%|
|S3 Intelligent Tier|99.999999999%|99.9%|
|S3 Infrequent access|99.999999999%|99.9%|
|S3 Onezone IA|99.999999999%|99.5%|
|S3 Glacier|99.999999999%|99.99% Configurable retrieval times from min to hrs|
|S3 Glacier Deep Archive|99.999999999%| retrieval times 12hrs|    
    
```
</p>
</details>

    
#### S3 - Security - Options for encrypting data stored on Amazon S3?
<details><summary>show</summary>
<p>

```    

You can choose to encrypt data using SSE-S3, SSE-C, SSE-KMS, or a client library such as the Amazon S3 Encryption Client. All four enable you to store sensitive data encrypted at rest in Amazon S3.

SSE-S3 provides an integrated solution where Amazon handles key management and key protection using multiple layers of security. You should choose SSE-S3 if you prefer to have Amazon manage your keys.

SSE-C enables you to leverage Amazon S3 to perform the encryption and decryption of your objects while retaining control of the keys used to encrypt objects. With SSE-C, you don’t need to implement or use a client-side library to perform the encryption and decryption of objects you store in Amazon S3, but you do need to manage the keys that you send to Amazon S3 to encrypt and decrypt objects. Use SSE-C if you want to maintain your own encryption keys, but don’t want to implement or leverage a client-side encryption library.

SSE-KMS enables you to use AWS Key Management Service (AWS KMS) to manage your encryption keys. Using AWS KMS to manage your keys provides several additional benefits. With AWS KMS, there are separate permissions for the use of the master key, providing an additional layer of control as well as protection against unauthorized access to your objects stored in Amazon S3. AWS KMS provides an audit trail so you can see who used your key to access which object and when, as well as view failed attempts to access data from users without permission to decrypt the data. Also, AWS KMS provides additional security controls to support customer efforts to comply with PCI-DSS, HIPAA/HITECH, and FedRAMP industry requirements.

Using an encryption client library, such as the Amazon S3 Encryption Client, you retain control of the keys and complete the encryption and decryption of objects client-side using an encryption library of your choice. Some customers prefer full end-to-end control of the encryption and decryption of objects; that way, only encrypted objects are transmitted over the Internet to Amazon S3. Use a client-side library if you want to maintain control of your encryption keys, are able to implement or use a client-side encryption library, and need to have your objects encrypted before they are sent to Amazon S3 for storage.

```
</p>
</details>

#### S3 - Security - How could I secure S3 objects from unintented user actions ?

<details><summary>show</summary>
<p>

```    
* S3 versioning  to preserve, retrieve, and restore every version of every object stored in your Amazon S3 bucket.
    
*  you can add an optional layer of security by enabling Multi-Factor Authentication (MFA) Delete for a bucket.7 With this option enabled for a bucket, two forms of authentication are required to change the versioning state of the bucket or to permanently delete an object version: valid AWS account credentials plus a sixdigit code (a single-use, time-based password) from a physical or virtual token device.

* To track requests for access to your bucket, you can enable access logging. Each access log record provides details about a single access request, such as the requester, bucket name, request time, request action, response status, and error
code, if any.
```
</p>
</details>


    
