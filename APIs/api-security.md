# API Security

- [What's the best approach for generating a new API key?](https://stackoverflow.com/questions/14412132/whats-the-best-approach-for-generating-a-new-api-key)
- [API Security: The Complete Guide](https://www.neuralegion.com/blog/api-security/)
- [OWASP Top Ten](https://owasp.org/www-project-api-security/)
- [Azure API Gateway tutorials](https://docs.microsoft.com/en-us/learn/modules/publish-manage-apis-with-azure-api-management/1-introduction)



## OWASP Top 10



## API Keys

- **Purpose**
  - Public facing key to uniquely identify the application making a request.
- **Characteristics**
  - Not suitable for identifying users
  - Can easily be stolen
- **Uses**
  - Rate limits
  - Consumption limits
  - Analytics