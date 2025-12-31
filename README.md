flowchart LR
    Dev[Developer Pushes Code] -->|git push| GitHub[GitHub Repository]

    GitHub -->|Triggers Workflow| GHA[GitHub Actions]

    GHA -->|OIDC Token| STS[AWS STS]
    STS -->|AssumeRoleWithWebIdentity| OIDC_ROLE[IAM OIDC Role]

    OIDC_ROLE -->|Temporary Credentials| GHA

    GHA -->|aws cloudformation deploy| CFN[AWS CloudFormation]

    CFN -->|Assume Role| CFN_ROLE[CloudFormation Execution Role]

    CFN_ROLE --> VPC[VPC / Network Stack]
    CFN_ROLE --> APP[Application Stack]
    CFN_ROLE --> IAM[IAM / Supporting Resources]

    subgraph GitHub Environments
        DEV[dev]
        STAGE[stage]
        PROD[prod]
    end

    GitHub --> DEV
    GitHub --> STAGE
    GitHub --> PROD
