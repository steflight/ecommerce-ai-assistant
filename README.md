# ecommerce-ai-assistant

graph TD
    %% Front
    A[Mobile/Web App] --> B[API Gateway / Auth<br/>(JWT, RBAC, Rate limit)]
    B --> C[Application API<br/>(Orchestrateur)]

    %% Governance & control
    C --> D[LLM Guardrails & Policy<br/>(modération, PII, anti-prompt)]
    C --> E[Intent Classifier (LLM)]
    C --> F[Query Planner<br/>(filtres + requête sémantique)]

    %% Search layer
    F --> G[Search Layer]
    G --> G1[Vector DB<br/>(Qdrant/Pinecone, HNSW)]
    G --> G2[Keyword Index<br/>(BM25 - OpenSearch/ES)]
    G --> G3[Reranker<br/>(cross-encoder)]

    %% Catalog & validation
    C --> H[Business Validation<br/>(prix, stock, règles marché)]
    H --> I[(Product Catalog DB)]
    I --> J[(Cache - Redis)]
    C --> K[Recommendations (optionnel)]

    %% LLM response composer
    C --> L[Response Composer (LLM)<br/>(sortie structurée JSON)]
    L --> M[API Response]
    M --> A

    %% Transverses
    subgraph N[Transversal]
      O[Observability<br/>(Logs/Metrics/Traces)]
      P[Secrets & Config<br/>(Vault/SSM)]
      Q[Provider Router LLM<br/>(OpenAI/Mistral/Anthropic)]
      R[Caching<br/>(résultats & prompts)]
    end

    %% Wires transverses
    C -.-> O
    B -.-> O
    G -.-> O
    C -.-> P
    C -.-> Q
    C -.-> R
