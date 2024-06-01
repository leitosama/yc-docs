# Example of using {{ message-queue-full-name }} on JMS

[JMS](https://www.oracle.com/technetwork/java/jms/index.html) is an API for sending messages between application components. With the AWS SQS Java Messaging Library, you can use {{ message-queue-name }} to send and receive messages via JMS.

## Installation {#install}

Install the AWS SDK for Java by [following the instructions](https://docs.aws.amazon.com/en_us/sdk-for-java/v1/developer-guide/setup-install.html) on the official website.

## Getting started {#prepare}

{% include [mq-http-api-preps](../_includes_service/mq-http-api-preps-sdk.md)%}

Set the environment variables:

```
export AWS_ACCESS_KEY_ID="<access_key_ID>"
export AWS_SECRET_ACCESS_KEY="<secret_key>"
```

Create a queue in {{ message-queue-name }} and prepare its URL.

## Example {#sample}
To run the example, add these dependencies:
```java
<dependencies>
    <dependency>
      <groupId>com.amazonaws</groupId>
      <artifactId>amazon-sqs-java-messaging-lib</artifactId>
      <version>1.0.8</version>
    </dependency>
    <dependency>
      <groupId>com.amazonaws</groupId>
      <artifactId>aws-java-sdk-sqs</artifactId>
      <version>1.11.176</version>
    </dependency>
  </dependencies>
```

In this example:

1. A connection with {{ message-queue-name }} is established.
1. A message queue is created.
1. A message with the text `test-message` is sent to the queue.
1. The message is read from the queue and displayed in the terminal.

To read about the features not covered by the example, see the documentation on the [AWS SQS Java Messaging Library](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-java-message-service-jms-client.html) website.

```java
package ru.yandex.cloud.message_queue;

import com.amazon.sqs.javamessaging.AmazonSQSMessagingClientWrapper;
import com.amazon.sqs.javamessaging.SQSConnection;
import com.amazon.sqs.javamessaging.SQSConnectionFactory;
import com.amazon.sqs.javamessaging.ProviderConfiguration;
import com.amazonaws.auth.AWSCredentials;
import com.amazonaws.auth.AWSStaticCredentialsProvider;
import com.amazonaws.services.sqs.AmazonSQSClientBuilder;
import com.amazonaws.client.builder.AwsClientBuilder.EndpointConfiguration;

import javax.jms.*;

public class App
{
    private static String queueName = "mq_jms_example";

    public static void main( String[] args ) throws JMSException
    {
        SQSConnectionFactory connectionFactory = new SQSConnectionFactory(
                new ProviderConfiguration(),
                AmazonSQSClientBuilder.standard()
                        .withRegion("{{ region-id }}")
                        .withEndpointConfiguration(new EndpointConfiguration(
                            "https://message-queue.{{ api-host }}",
                            "{{ region-id }}"
                        ))
        );

        SQSConnection connection = connectionFactory.createConnection();

        AmazonSQSMessagingClientWrapper client = connection.getWrappedAmazonSQSClient();

        if( !client.queueExists(queueName) ) {
            client.createQueue( queueName );
        }

        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);

        Queue queue = session.createQueue(queueName);

        MessageProducer producer = session.createProducer(queue);

        Message message = session.createTextMessage("test message");
        producer.send(message);

        MessageConsumer consumer = session.createConsumer(queue);
        connection.start();
        message = consumer.receive(1000);
        System.out.println(((TextMessage) message).getText());
    }
}
```
