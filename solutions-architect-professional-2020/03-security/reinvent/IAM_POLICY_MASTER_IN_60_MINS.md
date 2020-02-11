# Become an IAM Policy Master in 60 Minutes or Less

## IAM Policy Language Recap

What are policies? 

Two parts:
- Specfication: Define access policies
- Enforcement: Evaluation


IAM Policy Structure
**PARC Effect**

- Effect: Allow/Deny
- Principal: attach these to principals like users or roles. The entity that is allowed access
- Action: action, Type of access that is allowed or denied access (+4000)
- Resource: The ARN of the resource(s) that you are allowing/denying access to
- Condition - Conditons under the access defined is valid

## Policy enforcement

- Decision starts at deny
- Evaluate all applicable policies
- Is there an explicit Deny?
    - Yes (Deny)
    - No move forward
- Is there an Allow
    - Yes Final decision Allow
    - No Final decision deny

## Context and Policies

Through Context of your request and Your defined policies matching determines if things are allowed or denied

