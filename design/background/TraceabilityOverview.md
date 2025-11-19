# **Traceability Overview**

# **Introduction**

Today’s personal data hungry AI-backed services promise great innovation, convenience and insight, but also serve to double down on the fundamental privacy and data governance challenges that we have still not solved in the pre-AI world. There is a growing range of services in finance, personal fitness, transportation, employment and healthcare all of which offer valuable insights driven by personal data. 

# **Traceability will help consumers, enterprises and regulators**

Amidst this excitement, consumers need to know that when we share data with third parties we can maintain accountability and control. For enterprises, the strategic value of data is only growing, increasing the need for data governance infrastructure that can scale across multi-enterprise data ecosystems. Shared analytics drive business partnerships, third-party data is essential to training more accurate, robust AI models. As business-to-business partnerships are increasingly enabled by valuable data assets, both data providers and recipients need mechanisms for governing this data according to contractual and regulatory requirements. Finally, regulators need a new investigative toolkit for the AI age. Arguably, most regulators have broad legal authority today to penalize harmful uses of AI and other data analytics, especially in regulated sectors. However, enforcement authorities are at a severe disadvantage because of their inability to detect violations in complex, analytics-driven decision making. 

# **OTrace Protocol**

In response to these needs, we have designed and prototyped a new data traceability protocol called OTrace. Ecosystems that implement OTrace enable all parties to answer three basic questions: 1\) Who has my data? 2\) Is the data being used according to agreed-upon rules (consents, laws, contracts, etc), and 3\) how can I exercise control over the data?

The core traceability experience enabled by OTrace provides three new affordances: a Data Receipt (\#dataReceipt) to help consumers be aware of who is using their data and how; a Traceability Agent which collects Data Receipts and analyzes along with \#attestations to detect  possible mis-use; and an ecosystem of Traceability Services, which provides an authoritative repository of \#attestations documenting all of personal data transactions including \#consents, Data Access Authorizations (\#dataAuthorizations). 

# **The Traceability Experience with OTrace**

Here’s what a basic traceability experience would look like with OTrace in place, step by step:

1. A consumer (data subject) decides to **share data** from a data provider (perhaps their bank) with a fintech that is offering some service such as a loan or advice on retirement planning.   
2. The consumer \#**introduces** the bank and the fintech to each other, **authorizes** access to certain personal data (\#dataAuthorization) and tells both the bank and the fintech where to **record \#attestations** of each step of the interaction in order to provide traceability. The \#attestations are recorded in a Traceability Service whose address is included in the \#introduction.  
3. The three parties agree on how they data can be used in a \#**consent**, which is also recorded as an attestation.  
4. The fintech uses the data to make a **decision** or provide a **service** to the consumer, providing a \#dataUse attestation and indicating the **basis** (ie consent or some other legal basis) for using the data.  
5. When the fintech announces a decision or action to the consumer in the form of a **\#dataReceipt**, which summarizes how the data was used and where the permission to use it came from.  
6. The consumers \#traceAgent analyzes the data receipt and helps the consumer determine whether the data was used properly.

The overall goal of OTrace is to give consumers and enterprises as much trust and confidence in the handling of our personal data as we have in the way financial institutions handle our money. Personal data is valuable and its misuse can be genuinely harmful. Traceability, as enabled by OTrace, increases trust and creates accountability at scale, allowing new data-driven AI services to be deployed rapidly with a baseline of trust.

# **Use Cases**

We can see how OTrace will work by playing around with these implemented use cases: a loan applications, and the use of driver location data to make insurance underwriting and ad placement decisions.

