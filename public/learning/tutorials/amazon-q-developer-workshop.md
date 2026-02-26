---
title: "Amazon Q Developer Workshop"
tags: [learning, tutorial, aws, amazon-q]
created: 2026-02-26
---

# Amazon Q Developer Workshop

[Amazon Q Developer - Getting Started from Build to Operate](https://catalog.us-east-1.prod.workshops.aws/workshops/86ecf440-3222-4879-836e-995e5919892c/en-US)
panyapoc.com/qhero
## Required Pre-workshop Setup
Please complete the following environment setup before the workshop:  
[Install Required Tools : Q CLI and Q IDE](https://catalog.us-east-1.prod.workshops.aws/workshops/86ecf440-3222-4879-836e-995e5919892c/en-US/2-setup-q/27-running-to-your-machine)

Choose one option for subscription: 
- Option1 :  [Create Builder ID](https://catalog.us-east-1.prod.workshops.aws/workshops/86ecf440-3222-4879-836e-995e5919892c/en-US/2-setup-q/24-create-an-aws-builder-id)
- Option2 :  [Configure AWS IAM Identity Center (Paid)](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/getting-started-idc.html)
## Intro to Q Developer

Key capabilities of Amazon Q Developer include:
- **Code generation** - Create functions, classes, and entire applications based on natural language descriptions
- **Code transformation** - Refactor, optimize, and migrate existing code
- **Technical Q&A** - Get answers to programming questions and AWS service inquiries
- **Documentation assistance** - Generate comments, documentation, and test cases
- **AWS integration** - Seamless assistance with AWS services, APIs, and best practices
- **Cloud operations** - Simplify AWS resource management and infrastructure tasks
Benefit
- **Accelerated Development** - Complete coding tasks in minutes instead of hours
- **Reduced Context Switching** - Get answers without leaving your development environment
- **Knowledge Expansion** - Learn new technologies and best practices as you work
- **Improved Code Quality** - Generate well-structured, documented code following best practices
- **AWS Expertise On Demand** - Access AWS knowledge without extensive documentation searches
- **Simplified Cloud Operations** - Streamline infrastructure management and troubleshooting

## Setup the workshop

## Lab 1 - Setting Up Amazon Q Developer
Configure Amazon Q in your development environment by creating an AWS Builder ID and setting up Amazon Q in both CLI and IDE environments.

Q CLI
```
q login
q chat
q chat --resume
/context show
/context add expert-software-engineer​.md
/profile create expert-software-engineer
/profile set default
/model
/tools
! run cli command within q session
```
Context ของ Amazon Q IDE คือ folder และ file ที่อยู่ด้านใน workspace ที่เราทำงาน
Context ของ Amazon Q CLI คือ  file ที่อยู่ด้านใน folder ที่เราเข้า `q chat`

เราอาจจะสร้าง Profile แยกสำหรับแต่ละงาน
- A "terraform" profile with infrastructure-as-code guidelines
- A "python" profile with Python coding standards
- A "java" profile with Java best practices
## Lab 2 - Creating a Basic and Enhanced Web Application
 Build a web application with Amazon Q's assistance using both CLI and IDE, generate documentation, and enhance the application with advanced prompting techniques.

Generate website:
```
create simple html sudoku web application and save into file
```
Generate Documentation
```
Could you create a README file for this application?
```
Debug
```
"Why isn't this code working as expected?"

describe your issue...
```
Improvement
```
"How can I improve the user experience of my Sudoku game?"
"Can you help me add a timer feature to my Sudoku game?"
"How can I save the game state so users can continue later?"
"How do I generate a valid Sudoku puzzle in JavaScript?"
"Write a function to check if a Sudoku solution is valid"
"How can I highlight conflicting numbers in my Sudoku grid?"
"Create a function to save the current game state to localStorage"
"How do I implement different difficulty levels for my Sudoku game?"
```

Principles of effective prompting with Amazon Q:

1. **Be specific and detailed**: Provide context, requirements, and constraints
2. **Break down complex tasks**: Ask for one feature at a time
3. **Iterative refinement**: Start with a basic implementation, then ask for improvements
4. **Provide examples**: When possible, show examples of what you want
5. **Ask for explanations**: Request explanations of how the code works

Prompt:
```
I want to enhance my Sudoku game UI with the following features:
1. A modern, clean design with a color scheme that reduces eye strain
2. A responsive layout that works well on both desktop and mobile devices. 
3. Verify grid layout display.
4. Visual feedback for selected cells, valid/invalid entries, and completed rows/columns/boxes
5. A game info panel showing:
   - Current difficulty level
   - Timer
   - Number of mistakes
   - Completion percentage
6. Animations for cell selection and number placement
7. Support for both mouse and keyboard input
```
performance optimization
```
I've implemented the enhanced Sudoku game, but I'm concerned about performance, especially:

1. The solver algorithm seems slow on difficult puzzles
2. There's a noticeable delay when validating the board after each input
3. The UI becomes less responsive when using the visualization feature

Please suggest optimizations to improve performance while maintaining functionality.
```
code quality
```
I want to improve the organization and maintainability of my Sudoku game code:

Current issues:
- The code has grown large and is becoming difficult to manage
- There's some duplication in the validation and generation logic
- Event handlers are getting complex with multiple responsibilities

Please suggest how I can:
1. Refactor the code using appropriate design patterns
2. Separate concerns (UI, game logic, state management)
3. Improve code readability and maintainability
4. Add proper error handling
```
## Lab 3 - Deploying with AWS CDK
Deploy your application to AWS using CloudFront and S3 by creating CDK infrastructure, deploying it, exploring the created resources, and generating architecture diagrams.
```
Use AWS Cloud Development Kit (CDK) to implement infrastructure as code in Python.
By using AWS solutions as follows:
- Host web application in Amazon S3 private bucket
- Deliver web through Amazon CloudFront.
```
```
Deploy this CDK Stack
```

Generate Architecture Diagram
```
please generate an architecture diagram for this application that I can load on drawio
```
Setting up a custom domain name
```
How do I configure a custom domain name for my CloudFront distribution using CDK?
```
Implementing CI/CD for automatic deployments
```
How can I set up a GitHub Actions workflow to automatically deploy my Sudoku app when I push changes?
```
Adding monitoring and analytics
```
How can I add monitoring and analytics to my deployed Sudoku application?
```
Clean Up
```
Delete all resources which created by this CDK Stack in my AWS Account
```
```
!rm -rf *
/clear 
/context clear
```
## Lab 4 - Simplify Cloud Operations with Amazon Q CLI
Streamline AWS cloud operations by exploring CloudFormation stacks, creating VPC flow logs, and analyzing network configurations with Amazon Q.

```
Create an IT inventory report for server resources in aws us-east-1 region. The report should contain:
* Resource type
* Instance type
* vCPU
* Memory (GB)
* Public IP
* Private IP
* OS
* Open port(s)
* IAM role
* Comments - based on AWS best practice

Output the report in markdown and html format.
```

```
Generate a comprehensive AWS VPC architecture diagram for VPC ID $VPC_ID🚨 in the $AWS_REGION. Include: 
* VPC and Subnets with CIDR
* Internet gateways 
* NAT gateways
* EC2 instances, RDS databases, and any other significant resources within the VPC including their IP (private and public)
* Do not need to provide resource association
Export the diagram in draw.io format.
```
  
---
Use Amazon Q to Query Default VPC Flow Logs 📊

```
What is top 20 traffic going thru NAT Gateway ENI
VPC: $VPC_ID
Region: $AWS_REGION
Query traffic from VPC flow log
Make sure to check log format to see the field
Output to traffic_table.csv
```

```
Download the full list AWS IP ranges source from https://ip-ranges.amazonaws.com/ip-ranges.json
then create a filtered list of AWS IP address.
* Exclude "service": "AMAZON"
* Filter IP for only this region
Output to filtered_aws_ips.csv
```

```
Tag IP in traffic table using filtered_aws_ips.csv to understand what IP is associated with
* VPC IP
* AWS Service Name
* Internet
Output to as csv traffic_tagged.csv
```

```
Using the traffic table and AWS Service IP table create a network visualization diagram. Node is IP address, with label IP, Tag. Edge is directed graph between src address and dst address, Edge weight based on total data transfer volume, Edge thickness proportional to data transfer volume. Use networkx, graphviz to visualize.
```

```
Can you group IPs with the same tag closer together
```

```
Set up S3 VPC Endpoint 
VPC: $VPC_ID
Region: $AWS_REGION
```

Advance VPC flowlog Analysis

```
Create new log group and VPC flowlog for $VPC_ID🚨 in $AWS_REGION
Capture at 1 min interval and store it in cloudwatch log.
Use log format that allow log to see traffic going thru NAT gateway, as well as which AWS service. 
Output the flowlog detail and VPC CIDR to a markdown file custom-flowlog.md
```

```
tail the vpc flowlog 
```

```
What is top 20 traffic by bytetrasfer
Focus on pkt-srcaddr, pkt-dstaddr,pkt-src-aws-service, pkt-dst-aws-service
Ignore srcaddr, dstaddr
Query traffic from VPC flow log detail in custom-flowlog.md
Make sure to check log format to see the field
Output to traffic_table.csv
```

```
Using the traffic_table.csv create a network visualization diagram. 
Node is IP address, with label is IP
Label
* VPC
* AWS services (Service name: S3, DynamoDB, etc.)
* Internet
Edge is directed graph between pkt-srcaddr and pkt-dstaddr
Edge weight based on total data transfer volume
Use networkx, graphviz to visualize.
```

Troubleshoot network issues with Q

```
Please help me find EC2 instances with names containing 'load-generator' in $AWS_REGION
```
```
I can't access the UI on port 80 in that EC2, 
please help me find the root cause
```
```
please suggest fix, also tell be about possible imapact of the fix. 
let me decide if I want to proceed
```

IT Automation with Q

Schedule EC2
```
Create an automation using AWS System Manager 
To schedule EC2 with names containing 'sampleapp-load-generator" in $AWS_region 
The EC2 should be running on Weekday, 9to5 BKK time. and stop on other slot
```
CloudTrail Analysis
```
Analyze 24 hours periods of CloudTrail data and compare them
This is minimum dimension I'm looking for 
1. Comprehensive patterns analysis
2. Anomaly detection
3. Actionable insights on what needs attention, 
All formatted in a clean, digestible report. 
please send it via sns topic named cloudtrail-report every morning 8 AM TH time. Add $YOUR_EMAIL as subscriber 
```
## Lab 5 - Strengthen Security with Amazon Q CLI
Perform comprehensive security assessments aligned with the AWS Well-Architected Framework's security pillar, analyze CloudFormation templates for vulnerabilities, and implement security best practices with Amazon Q's guidance.

```
Please summarize security review for this AWS account in N.Virginia region, summarize in file security-review.html 
show resources detail, use AWS Well-Architected Review Framework Security latest version for guideline. 
Topic in this report is for solutions architect that including executive summary - security pillar assessment 
summary, finding, analysis sort by prioritize, recommendation. The html file report using CloudScape as template 
the web page must look clean, professional and easy to read and aware about adjust correct indent.

```
## Lab 6 - Financial Optimization with Amazon Q CLI
Learn how to optimize costs and improve financial efficiency in your AWS environment using Amazon Q's recommendations for resource sizing, utilization analysis, and cost-saving opportunities.

```
Create a comprehensive AWS cost optimization report for my current AWS account with the following requirements:

1. **Cost Analysis & Data Collection**:
   - Analyze my current AWS account costs using get_pricing_from_api only
   - Get detailed cost breakdown by service for the last 3 months
   - Identify cost trends, increases, and optimization opportunities
   - Calculate potential savings with specific recommendations

2. **Generate Two Report Formats**:
   - **Markdown Report**: Detailed technical analysis with cost breakdowns, recommendations, and action items
   - **HTML Report**: Interactive dashboard using CloudScape design framework

3. **HTML Report Requirements**:
   - Use AWS CloudScape design system for professional AWS-native styling
   - Integrate Chart.js for interactive data visualizations including:
     * Overall monthly cost trend line chart
     * Service cost breakdown doughnut/pie chart
     * Month-over-month cost changes bar chart
     * Top 5 services multi-line trend chart
   - Include executive summary with key metrics
   - Color-coded service cards showing cost trends and optimization levels
   - Detailed action items with priority levels (High/Medium/Low)
   - Professional table layouts for detailed cost analysis
   - Aware about adjust correct indent

4. **Key Analysis Components**:
   - Identify services with significant cost increases (>50%)
   - Calculate potential savings from Reserved Instances
   - Provide immediate, short-term, and long-term recommendations
   - Include cost optimization best practices
   - Highlight critical alerts for services requiring immediate attention

5. **Visualization Requirements**:
   - Use real AWS cost data from my account
   - Interactive tooltips with detailed cost information
   - Color-coded trends (red for increases, green for decreases)
   - Responsive design that works on different screen sizes
   - Professional AWS color palette and typography

6. **Output Files**:
   - Save markdown report as `aws_cost_optimization_report.md`
   - Save HTML report as `aws_cost_optimization_report.html` in my current directory
   - Ensure HTML file includes all necessary CDN links for Chart.js and CloudScape

The final deliverable should be a production-ready cost optimization dashboard that provides both high-level insights and actionable recommendations for immediate cost savings.
```
## Lab 7 - Enhancing Amazon Q with Context
Learn how to customize Amazon Q's behavior using context files and profiles to align with your specific development preferences, coding standards, and architectural patterns.

```
# Business Context
My company would like to develop a flappy bird like web application with the following features
- Web application support responsive and playable on multiple devices (including PC and mobile web browsers)
- Player can select 3 levels to play: easy, medium and hard. The shorter space, the harder level.
    - Player must select level before playing game
- Collect High Score
    - Provide button to view high score on level selection screen
    - Separate high score screen for each level
- Display game play stats on top left of screen including
    - Number of pipes passed
    - Time played
- Display the background in black (like playing in space)
- Player icon is UFO

# Technical Details
- Implement using simple HTML, Javascript and CSS
- Store high score on local storage

# Request/Ask
- You are a senior developer, your task is to create a flappy bird like web application to realize business context and technical details explained above.
- You would like to ensure the code is clean and follow best practices for HTML, JavaScript and CSS development

# Output Format
- Generate HTML, JavaScript and CSS files in folder flappy-bird-no-context folder
- Please note and update your plan in scratchpad.md 
- Also update README.md for project summary and installation instructions
```

```expert-software-engineer​.md
# Profile: Expert Software Engineer​

# Role​
I am a senior software engineer experienced in design and building web and backend application​

# Thought and Working Processes​
- I analyze the business requirements and think through the solutions based on my experiences and expertise. Always consider benefits and trade offs before creating and implementing the solutions​
- I always apply software engineering best practices to my code including
    - Clean Architecture - Start from domain model and surrounded with application and infrastructure layers using inversion of control principle.​
```

```commonrule.md
# Rules / Important Instruction​

- THINK THROUGH THE PROCESS STEP BY STEP​.
- WRITE DOWN YOUR THOUGHT PROCESS IN scratchboard.md​ IN YOUR GENERATED FOLDER.
- WRITE DOWN MY PROMPTS INTO prompts.md WITH A TIMESTAMP​ IN YOUR GENERATED FOLDER.
- MUST FOLLOW CONTEXT PROFILE AND START GENERATE CONTENT BY FOLLOW CONTEXT AND ADD NOTE IN scratchboard.md.
- IF YOU ASK ANY QUESTIONS, PLEASE SUMMARIZE THEM IN MAXIMUM 5 QUESTION AT A TIME AND THEN ASK EACH OF THEM ONE BY ONE AGAIN IF THE USER IS NOT ANSWERING THEM ALL AT ONCE.​
```
    
**Code Challenge**: 

Test your skills by building a shopping cart API for an e-commerce system following clean architecture and TDD principles using AWS services like Lambda, API Gateway, and DynamoDB and win the prize!!! 🏆 🎁

```
As a customer, I want to add items to my shopping cart
So that I can collect items I intend to purchase

Acceptance Criteria:
* I can add an item with specified quantity to my cart
* The cart should display the updated item count
* The cart should show the item details (product name, price, quantity)
* The cart should calculate and display the subtotal
```

```
I am a developer, I want to create a service to manage shopping cart API for e-commerce system. 
I want to start with "add items to cart features".

Below is the user story with acceptance criteria for this feature.
As a customer, I want to add items to my shopping cart
So that I can collect items I intend to purchase

Acceptance Criteria:
* I can add an item with specified quantity to my cart
* The cart should display the updated item count
* The cart should show the item details (product name, price, quantity)
* The cart should calculate and display the subtotal

There are some technical requirements as follows 
* Programming language is Python
* Follow Clean Architecture approach.
* Follow Test-Driven Development (TDD) approach. Enforce test first and follow red, green and refactor mechanism.
* Use AWS Cloud Development Kit (CDK) to implement infrastructure as code in Python
* Separate infrastructure and application folder. Infrastructure folder will be used to host the CDK code
* Use the following AWS solutions for build APIs: API Gateway, Lambda and DynamoDB

Please only do 1 feature at a time, start with add item to cart. 
Also note the plan and update progress on scratchboard.md
```

```
Could you help building and deploying this application? Please also update progress on scratchboard.md.
```

```
Could you help running the API test? Please also update progress on scratchboard.md.
```

```
Could you create a README file for this application? Please also update progress on scratchboard.md.
```

```
Based on current code, I would like to add additional feature to shopping cart API.

Below is the user story for this feature
As a customer, I want to remove items from my shopping cart
So that I can adjust my intended purchases

Acceptance Criteria:
* I can remove a specific item completely from the cart
* The cart should update the total item count
* The cart should recalculate and display the updated subtotal

Remember to keep the technical requirements as previously mentioned.
Only do 1 feature at a time. 

Also note the plan and update progress on scratchboard.md
```

```
Delete all resources which created by this CDK Stack in my AWS Account
```
## Tips and Tricks

- Start slow, focus on a specific task, understand the response from Amazon Q Developer. Stop, verify and respond to polish the result if something doesn't feel right :).
- Provide detailed contexts to Amazon Q Developer. The more you provide, the more accurate results you will get.
- Keep the plan and progress updated, ensure you tell Amazon Q Developer to note down the plan and progress on scratchboard.md. This will help both you and Amazon Q Developer track the progress and get more accurate result.

## Troubleshooting

If you encounter problems inside the project like misconfiguration, packages or dependencies mismatch, etc... , feel free to start over by deleting the code and type `/clear` on CLI to restart the session.

Note that if you get an error after deploying application to AWS, you may need to delete CloudFormation stack or apply terraform plan to start over again.

### [Model Selection and Configuration](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/en-US/workshop/98-q-tips#model-selection-and-configuration)

**Tip 1: Dynamic Model Switching During Active Sessions**
Switch models mid-conversation using `/model` followed by arrow key selection:
```
Amazon Q> /model
? Select a model ›
❯ claude-4-sonnet
  claude-3.7-sonnet (active)
  claude-3.5-sonnet
```
This allows transitioning from Claude 3.5 Sonnet for routine tasks to Claude Sonnet 4 for complex architectural decisions within the same session.

**Tip 2: Model-Specific Command Usage**
```
q chat --model claude-4-sonnet
```
Ideal for sessions requiring advanced reasoning from the start, such as designing distributed systems.

**Tip 3: Setting Default Model Preferences**
```
q settings chat.defaultModel claude-4-sonnet
```
Ensures all new sessions default to Claude Sonnet 4 while maintaining override capability.

**Tip 4: Understanding Model Capabilities**
### [Image Support](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/en-US/workshop/98-q-tips#image-support)

**Tip 5: Supported Image Formats and Limitations**
```
Drag-and-drop architecture.png into terminal
Amazon Q> Generate Terraform for this design
```
Processes JPEG/PNG/WEBP/GIF up to 10 images per request.

**Tip 6: Architecture Diagram to Infrastructure as Code**
Convert visual designs to CloudFormation:
```bash
q chat
Amazon Q> [drag infrastructure-diagram.png] Create SAM template
Generates deployable templates from visual inputs in [upload design-mockup.jpg] Create responsive NavBar
```
Outputs JSX/CSS with accessibility features.

**Tip 7: Visual Documentation Analysis** 
Explain ER diagrams:
```
q chat
Amazon Q> [drop er-diagram.png] Generate SQL schema
```
Produces normalized database schemas from visual inputs.
### [Conversation Management and Persistence](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/en-US/workshop/98-q-tips#conversation-management-and-persistence)
**Tip 8: Conversation Persistence with Resume Functionality**
Maintain project-specific history:
```
cd ~/projects/inventory-service
q chat --resume
```
Restores previous session context when returning to directory.

**Tip 9: Manual Conversation State Management**
Archive important discussions:
```
Amazon Q> /save design-review-convo
Amazon Q> /load design-review-convo
```
Preserves critical decision-making threads.

**Tip 10: Directory-Based Context Switching**
Automatic context separation:
```
cd ~/projectA && q chat --resume  # Loads ProjectA context
cd ~/projectB && q chat --resume  # Loads ProjectB context
```
Prevents cross-project contamination
### [Command Line Integration and Autocompletion](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/en-US/workshop/98-q-tips#command-line-integration-and-autocompletion)
**Tip 11: Comprehensive CLI Autocompletion**
Get AWS CLI suggestions:
```
aws s3 cp 
Suggested: s3://bucket-name/path/
```
Supports 100+ tools including kubectl and terraform.

**Tip 12: Natural Language to Shell Translation**
Convert English to executable code:
```
q translate "rotate S3 logs daily with CloudWatch"
```
Outputs complete Lambda + EventBridge solution.

**Tip 13: Inline Terminal Suggestions**
Accelerate Docker workflows:
```
docker compose 
Suggested: up, down, build, config
```
Real-time syntax help as you type.
### [Model Context Protocol MCP and Extensibility](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/en-US/workshop/98-q-tips#model-context-protocol-mcp-and-extensibility)

[AWS MCP Server](https://awslabs.github.io/mcp/)  is a suite of specialized MCP servers that help you get the most out of AWS.

```
curl -LsSf https://astral.sh/uv/install.sh | sh
uv python install 3.10
```

`~/.aws/amazonq/mcp.json`
```
mkdir -p ~/.aws/amazonq

# Remove existing mcp.json file if it exists
rm -f ~/.aws/amazonq/mcp.json

# Create mcp.json file with the specified content
cat > ~/.aws/amazonq/mcp.json << 'EOF'
{
  "mcpServers": {
    "awslabs.core-mcp-server": {
      "command": "uvx",
      "args": [
        "awslabs.core-mcp-server@latest"
      ],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "autoApprove": [],
      "disabled": false
    }
  }
}
EOF

echo "MCP configuration file created at ~/.aws/amazonq/mcp.json"
```
## What You've Learned

- **Setting up Amazon Q Developer**: You've configured Amazon Q in both your IDE and CLI environments, giving you AI assistance across your development workflow.
- **AI-Assisted Development**: You've experienced firsthand how Amazon Q can help generate code, answer technical questions, and provide contextual recommendations as you build applications.
- **Web Application Creation**: You've built a basic web application with Amazon Q's assistance, learning effective prompting techniques to get the most helpful responses.
- **Advanced Prompting Skills**: You've mastered more sophisticated ways to interact with Amazon Q to solve complex development challenges and improve code quality.
- **Cloud Deployment with CDK**: You've deployed your application to AWS using infrastructure as code principles with CDK, setting up CloudFront and S3 for hosting.
- **Cloud Operations**: You've learned how Amazon Q CLI can simplify AWS resource management, help analyze CloudFormation stacks, implement VPC flow logs, and optimize network configurations.
- **Security Enhancement**: You've discovered how to use Amazon Q to strengthen your AWS account security by performing comprehensive security assessments, analyzing templates for vulnerabilities, and implementing best practices aligned with the AWS Well-Architected Framework.
- **Financial Optimization**: You've learned how to leverage Amazon Q to identify cost-saving opportunities, optimize resource utilization, and implement financial best practices to maximize the efficiency of your AWS environment.
- **Context-Aware Development**: You've explored how to enhance Amazon Q's capabilities using context files and profiles to customize its behavior according to your specific development preferences, coding standards, and architectural patterns.
- **Practical Problem-Solving**: Through the coding challenge, you've applied your new skills to build a shopping cart API following clean architecture and TDD principles using AWS services like Lambda, API Gateway, and DynamoDB.
## Next Steps

• Continue exploring Amazon Q Developer in your daily workflow 
• Experiment with different prompting techniques for various development tasks 
• Apply clean architecture and TDD principles in your projects with Amazon Q's assistance 
• Use Amazon Q to simplify your cloud operations and infrastructure management 
• Implement security best practices using Amazon Q's security assessment capabilities 
• Create custom context files and profiles to tailor Amazon Q's behavior to your specific needs 
• Share your experience and knowledge with your team 
• Stay updated on new Amazon Q features and capabilities

`Cloud-First to GenAI-First`

`https://www.promptz.dev/`
