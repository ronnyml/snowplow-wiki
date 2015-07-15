### 1. Create a Bintray account

You can do this on the Bintray homepage.

Once the account is created, navigate to the automatically created maven repository at bintray.com/{{username}}/maven, choose "edit", and tick the "GPG sign uploaded files using Bintray's public/private key pair" box.

(Note: it might be that you instead end up needing to use a keypair generated yourself as described [here][key-generation]).

### 2. Create a Sonatype account and a repository

This is documented in their [OSSRH Guide][ossrh-guide].

* [Create an account][create-account]
* [Create a repository creation ticket][create-ticket]

Further steps are available on the [Bintray blog][gateway-to-maven-central] and will be added to this page as they are attempted.

[ossrh-guide]: http://central.sonatype.org/pages/ossrh-guide.html
[create-account]: https://issues.sonatype.org/secure/Signup!default.jspa
[create-ticket]: https://issues.sonatype.org/secure/CreateIssue.jspa?issuetype=21&pid=10134
[gateway-to-maven-central]: http://blog.bintray.com/2014/02/11/bintray-as-pain-free-gateway-to-maven-central/
[key-generation]: http://www.apache.org/dev/openpgp.html#generate-key
