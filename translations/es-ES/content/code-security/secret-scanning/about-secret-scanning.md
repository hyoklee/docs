---
title: About secret scanning
intro: '{% data variables.product.product_name %} scans repositories for known types of secrets, to prevent fraudulent use of secrets that were committed accidentally.'
product: '{% data reusables.gated-features.secret-scanning-partner %}'
redirect_from:
  - /github/administering-a-repository/about-token-scanning
  - /articles/about-token-scanning
  - /articles/about-token-scanning-for-private-repositories
  - /github/administering-a-repository/about-secret-scanning
  - /code-security/secret-security/about-secret-scanning
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
type: overview
topics:
  - Secret scanning
  - Advanced Security
---

{% data reusables.secret-scanning.beta %}
{% data reusables.secret-scanning.enterprise-enable-secret-scanning %}

## About {% data variables.product.prodname_secret_scanning %}

If your project communicates with an external service, you might use a token or private key for authentication. Tokens and private keys are examples of secrets that a service provider can issue. If you check a secret into a repository, anyone who has read access to the repository can use the secret to access the external service with your privileges. We recommend that you store secrets in a dedicated, secure location outside of the repository for your project.

{% data variables.product.prodname_secret_scanning_caps %} will scan your entire Git history on all branches present in your {% data variables.product.prodname_dotcom %} repository for secrets{% ifversion ghec or ghes > 3.4 or ghae > 3.4 %}, even if the repository is archived{% endif %}.

{% ifversion fpt or ghec %}
{% data variables.product.prodname_secret_scanning_caps %} is available on {% data variables.product.prodname_dotcom_the_website %} in two forms:

1. **{% data variables.product.prodname_secret_scanning_partner_caps %}.** Runs automatically on all public repositories. Any strings that match patterns that were provided by secret scanning partners are reported directly to the relevant partner.

2. **{% data variables.product.prodname_secret_scanning_GHAS_caps %}.** {% ifversion fpt %}Organizations using {% data variables.product.prodname_ghe_cloud %} with a license for {% data variables.product.prodname_GH_advanced_security %} can enable and configure additional scanning for repositories owned by the organization.{% elsif ghec %}You can enable and configure additional scanning for repositories owned by organizations that use {% data variables.product.prodname_ghe_cloud %} and have a license for {% data variables.product.prodname_GH_advanced_security %}.{% endif %} Any strings that match patterns provided by secret scanning partners, by other service providers, or defined by your organization, are reported as alerts in the "Security" tab of repositories. If a string in a public repository matches a partner pattern, it is also reported to the partner.{% endif %}{% ifversion fpt %} For more information, see the [{% data variables.product.prodname_ghe_cloud %} documentation](/enterprise-cloud@latest/code-security/secret-security/about-secret-scanning#about-secret-scanning-for-advanced-security).{% endif %}

Service providers can partner with {% data variables.product.company_short %} to provide their secret formats for scanning. {% data reusables.secret-scanning.partner-program-link %}

{% ifversion secret-scanning-push-protection %}

You can also enable {% data variables.product.prodname_secret_scanning %} as a push protection for a repository or an organization. When you enable this feature, {% data variables.product.prodname_secret_scanning %} prevents contributors from pushing code with a detected secret. To proceed, contributors must either remove the secret(s) from the push or, if needed, bypass the protection. {% ifversion push-protection-custom-link-orgs %}Admins can also specify a custom link that is displayed to the contributor when a push is blocked; the link can contain resources specific to the organization to aid contributors. {% endif %}For more information, see "[Protecting pushes with {% data variables.product.prodname_secret_scanning %}](/code-security/secret-scanning/protecting-pushes-with-secret-scanning)."

{% endif %}

{% ifversion fpt or ghec %}
## About {% data variables.product.prodname_secret_scanning_partner %}

When you make a repository public, or push changes to a public repository, {% data variables.product.product_name %} always scans the code for secrets that match partner patterns. If {% data variables.product.prodname_secret_scanning %} detects a potential secret, we notify the service provider who issued the secret. The service provider validates the string and then decides whether they should revoke the secret, issue a new secret, or contact you directly. Their action will depend on the associated risks to you or them. For more information, see "[Supported secrets for partner patterns](/code-security/secret-scanning/secret-scanning-patterns#supported-secrets-for-partner-patterns)."

You cannot change the configuration of {% data variables.product.prodname_secret_scanning %} on public repositories.

{% ifversion fpt %}
{% note %}

{% data reusables.secret-scanning.fpt-GHAS-scans %}

{% endnote %}
{% endif %}

{% endif %}

{% ifversion not fpt %}

{% ifversion ghec %}
## About {% data variables.product.prodname_secret_scanning_GHAS %}
{% elsif ghes or ghae %}
## About {% data variables.product.prodname_secret_scanning %} on {% data variables.product.product_name %}
{% endif %}

{% data variables.product.prodname_secret_scanning_GHAS_caps %} is available on all organization-owned repositories as part of {% data variables.product.prodname_GH_advanced_security %}. It is not available on user-owned repositories. When you enable {% data variables.product.prodname_secret_scanning %} for a repository, {% data variables.product.prodname_dotcom %} scans the code for patterns that match secrets used by many service providers. {% ifversion secret-scanning-backfills %}{% data variables.product.prodname_dotcom %} will also periodically run a full git history scan of existing content in {% data variables.product.prodname_GH_advanced_security %} repositories where {% data variables.product.prodname_secret_scanning %} is enabled, and send alert notifications following the {% data variables.product.prodname_secret_scanning %} alert notification settings. {% endif %}For more information, see "{% ifversion ghec %}[Supported secrets for advanced security](/code-security/secret-scanning/secret-scanning-patterns#supported-secrets-for-advanced-security){% else %}[{% data variables.product.prodname_secret_scanning_caps %} patterns](/code-security/secret-scanning/secret-scanning-patterns){% endif %}."

If you're a repository administrator you can enable {% data variables.product.prodname_secret_scanning_GHAS %} for any repository{% ifversion ghec or ghes > 3.4 or ghae > 3.4 %}, including archived repositories{% endif %}. Organization owners can also enable {% data variables.product.prodname_secret_scanning_GHAS %} for all repositories or for all new repositories within an organization. For more information, see "[Managing security and analysis settings for your repository](/github/administering-a-repository/managing-security-and-analysis-settings-for-your-repository)" and "[Managing security and analysis settings for your organization](/organizations/keeping-your-organization-secure/managing-security-and-analysis-settings-for-your-organization)."

{% ifversion ghes or ghae or ghec %}You can also define custom {% data variables.product.prodname_secret_scanning %} patterns for a repository, organization, or enterprise. For more information, see "[Defining custom patterns for {% data variables.product.prodname_secret_scanning %}](/code-security/secret-security/defining-custom-patterns-for-secret-scanning)."
{% endif %}

{% ifversion secret-scanning-ghas-store-tokens %}
{% data variables.product.company_short %} stores detected secrets using symmetric encryption, both in transit and at rest.{% endif %}{% ifversion ghes > 3.7 %} To rotate the encryption keys used for storing the detected secrets, you can contact {% data variables.contact.contact_ent_support %}.{% endif %}

### About {% data variables.product.prodname_secret_scanning %} alerts

When you enable {% data variables.product.prodname_secret_scanning %} for a repository or push commits to a repository with {% data variables.product.prodname_secret_scanning %} enabled, {% data variables.product.prodname_dotcom %} scans the contents of those commits for secrets that match patterns defined by service providers{% ifversion ghes or ghae or ghec %} and any custom patterns defined in your enterprise, organization, or repository{% endif %}. {% ifversion secret-scanning-backfills %}{% data variables.product.prodname_dotcom %} also periodically runs a scan of all historical content in repositories with {% data variables.product.prodname_secret_scanning %} enabled.{% endif%}

If {% data variables.product.prodname_secret_scanning %} detects a secret, {% data variables.product.prodname_dotcom %} generates an alert.

- {% data variables.product.prodname_dotcom %} sends an email alert to the repository administrators and organization owners. You'll receive an alert if you are watching the repository, or if you have enabled notifications for security alerts, or for all the activity on the repository.
{% ifversion ghes or ghae or ghec %}
- If the contributor who committed the secret isn't ignoring the repository, {% data variables.product.prodname_dotcom %} will also send an email alert to the contributor. The emails contains a link to the related {% data variables.product.prodname_secret_scanning %} alert. The commit author can then view the alert in the repository, and resolve the alert.
{% endif %}
- {% data variables.product.prodname_dotcom %} displays an alert in the "Security" tab of the repository.

{% ifversion ghes or ghae or ghec %}
For more information about viewing and resolving {% data variables.product.prodname_secret_scanning %} alerts, see "[Managing alerts from {% data variables.product.prodname_secret_scanning %}](/github/administering-a-repository/managing-alerts-from-secret-scanning)."{% endif %}

Repository administrators and organization owners can grant users and teams access to {% data variables.product.prodname_secret_scanning %} alerts. For more information, see "[Managing security and analysis settings for your repository](/github/administering-a-repository/managing-security-and-analysis-settings-for-your-repository#granting-access-to-security-alerts)."

{% ifversion ghec or ghes or ghae > 3.4 %}
You can use the security overview to see an organization-level view of which repositories have enabled {% data variables.product.prodname_secret_scanning %} and the alerts found. For more information, see "[Viewing the security overview](/code-security/security-overview/viewing-the-security-overview)."
{% endif %}

{%- ifversion ghec or ghes or ghae %}You can also use the REST API to 
monitor results from {% data variables.product.prodname_secret_scanning %} across your {% ifversion ghec %}private {% endif %}repositories{% ifversion ghes %} or your organization{% endif %}. For more information about API endpoints, see "[{% data variables.product.prodname_secret_scanning_caps %}](/rest/reference/secret-scanning)."{% endif %}

{% endif %}

## Further reading

- "[Securing your repository](/code-security/getting-started/securing-your-repository)"
- "[Keeping your account and data secure](/github/authenticating-to-github/keeping-your-account-and-data-secure)"
{%- ifversion fpt or ghec %}
- "[Managing encrypted secrets for your codespaces](/codespaces/managing-your-codespaces/managing-encrypted-secrets-for-your-codespaces)"{% endif %}
{%- ifversion fpt or ghec or ghes %}
- "[Managing encrypted secrets for Dependabot](/code-security/supply-chain-security/keeping-your-dependencies-updated-automatically/managing-encrypted-secrets-for-dependabot)"{% endif %}
- "[Encrypted secrets](/actions/security-guides/encrypted-secrets)"
