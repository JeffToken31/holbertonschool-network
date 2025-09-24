# What happens when you type <https://www.google.com> and hit Enter?

## Check out my detailed blog post explaining what happens when you type `<https://www.google.com>` in your browser: [Read the full article on Medium](https://medium.com/@jeffrey-31/what-happens-when-you-type-https-www-google-com-and-hit-enter-d19588143dc4)

![Sequence flow of typing Google](https://github.com/JeffToken31/holbertonschool-network/blob/main/what_happens_when_your_type_google_com_in_your_browser_and_press_enter/googleFlow.png)

## Blog post content

Most of us just type, press Enter, and boom — Google is there in a blink. But under the hood, there's a lot of moving parts working in milliseconds. Let's break it down step by step.

1.DNS Request
Your browser first asks: "What's the IP of <www.google.com>?"  
The DNS system answers with something like 142.250.xx.xx. Think of it as looking up a phone number in a global directory.

2.TCP/IP Connection
With the IP in hand, your computer now sets up a connection. Packets travel across your local network, your ISP, and then jump through the internet backbone until they reach Google.

3.Firewalls
Along the way, firewalls check your request. They act like security guards: "Is this traffic safe? Port 443 only? Okay, pass."

4.HTTPS / SSL
Since it's HTTPS, your browser and Google exchange keys in a TLS handshake. Certificates are verified, encryption is enabled — so nobody can read your traffic in between.

5.Load Balancer
When your request lands at Google's infrastructure, a load balancer decides: "Which server should handle this?" This keeps traffic smooth and prevents overload.

6.Web Server
The chosen server (often Nginx or Apache) takes the request. Static stuff like images or CSS may be served directly. Anything dynamic gets passed along.

7.Application Server
Here's the brain: Google's application servers run the actual logic, like search. They might need to fetch fresh data before generating the response.

8.Database
When needed, the application servers query huge distributed databases. That's where indexes, user data, and cached results live.

From DNS lookups to encrypted traffic, load balancers, servers, and databases — it's a whole orchestra playing in sync just so that, when you type a URL, a page shows up almost instantly.

```mermaid
---
config:
  layout: elk
---
flowchart TD
 subgraph DC["Google Data Center"]
        K["Web Server Nginx or Apache"]
        J["Google Load Balancer"]
        L["Application Server Google Services"]
        M["Database Cluster: user data + index"]
  end
    A["Client Browser: user types google.com"] --> B["Local Firewall"]
    B -- "DNS Query: ask IP of google.com" --> C["Local DNS Resolver ISP"]
    C -- If not cached --> D["Root DNS Server"]
    D --> E["TLD DNS Server .com"]
    E --> F["Authoritative DNS google.com"]
    F -- "Returns IP 142.250.x.x" --> C
    C -- "Response google.com = 142.250.x.x" --> B
    B -- DNS Response allowed --> A
    A -- "HTTPS Request to 142.250.x.x port 443" --> B
    B -- Firewall check port 443 --> G["ISP Router"]
    G --> H["Internet Backbone"] & B
    H --> I["Google Firewall"] & G
    I --> J & H
    J --> K & I
    K --> L & J
    L --> M & K
    M --> L
    B -- HTTPS Response allowed --> A
    style A fill:#BBDEFB,stroke:#1565C0,stroke-width:2px,color:#0D47A1
    style B fill:#FFE0B2,stroke:#E65100,stroke-width:2px,color:#BF360C
    style C fill:#C8E6C9,stroke:#2E7D32,stroke-width:1.5px,color:#1B5E20
    style D fill:#C8E6C9,stroke:#2E7D32,stroke-width:1.5px,color:#1B5E20
    style E fill:#C8E6C9,stroke:#2E7D32,stroke-width:1.5px,color:#1B5E20
    style F fill:#C8E6C9,stroke:#2E7D32,stroke-width:1.5px,color:#1B5E20
    style G fill:#E1BEE7,stroke:#6A1B9A,stroke-width:1.5px,color:#4A148C
    style H fill:#E1BEE7,stroke:#6A1B9A,stroke-width:1.5px,color:#4A148C
    style I fill:#FFECB3,stroke:#F57F17,stroke-width:1.5px,color:#E65100
    style J fill:#FFECB3,stroke:#F57F17,stroke-width:1.5px,color:#E65100
    style K fill:#C5CAE9,stroke:#283593,stroke-width:1.5px,color:#1A237E
    style L fill:#FFF59D,stroke:#FBC02D,stroke-width:1.5px,color:#F57F17
    style M fill:#FFCDD2,stroke:#C62828,stroke-width:2px,color:#B71C1C
    style DC fill:#F0F4C3,stroke:#827717,stroke-width:2px,color:#3E2723

```
