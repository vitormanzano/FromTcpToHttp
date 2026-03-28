# CH2 - TCP

## L1: TCP
<p> 
    Transmission Control Protocol is a primary communication protocol of the internet. 
    TCP is great because it allows orderer data to be safely sent across the internet
</p>
| Text     | Binary      |
| -------- | -------     |
| i        | 01101001    |
| a        | 01100001    |
| m        | 01101101    |                 
| l        | 01101100    |
| i        | 01101001    |
| v        | 01110110    |
| e        | 01100101    |

<p>
    When data is sent over a network, it's sent in packets. 
    Each message is split into packets, the packets are sent, they arrive (pottentually) out of order,
    and ther are reassembled on the other side. Without a protocol like TCP, you can't guarantee that
    the order is correct. TCP solves this problem!
</p>
<p> 
    OBS: When the sender send a data, the receives need to send a ack (Acknoledgement) that he receives the data,
    the sliding window can proceed to the other packages. (He sent all the packages in the sliding window when receives the ack,
    after this se proceed to the other packages)
</p>
## L2: TCP vs. UDP
<ul>
    <li>
        TCP: Ensures that all the data is sent in order.
    </li>
    <li>
        UDP yeets the data to the receiver and hopes they can make sense of it
    </li>
</ul>

## Files vs. Network

<p>
    Files and network connections behave very similarly - files and network connections are both just streams of bytes that you can read from and write to.
</p>

<p>
    When you read from a file, you're in control of the reading process. You pull data from the file.
</p>

<p>
    When you read from a network connection, the data is pushed to you by the remote Server.
    You don't have control over when the data arrives, how much arrives, or when it stops arriving.
    Your code has to be ready to receive it when it comes
</p>
