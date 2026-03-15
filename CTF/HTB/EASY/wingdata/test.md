graph TD
    Root(ad.airevolution.cat) --> ADM[ADMINISTRACION]
    Root --> USR[USUARIOS]
    Root --> EQP[EQUIPOS]
    Root --> SRV[SERVIDORES]
    Root --> GRP[GRUPOS]
    Root --> REC[RECURSOS]

    USR --> U1[Administradores_Sistema]
    USR --> U2[Ingenieros_AI]
    USR --> U3[Marketing_Digital]
    USR --> U4[Ventas]
    USR --> U5[RRHH]
    USR --> U6[Atencion_Cliente]
    USR --> U7[Secretaria]

    EQP --> CLI[CLIENTES]
    CLI --> W[WINDOWS]
    CLI --> L[LINUX]
    EQP --> VPN[VPN_CLIENTS]
    EQP --> TST[EQUIPOS_TEST]

    SRV --> S1[INFRAESTRUCTURA]
    SRV --> S2[SERVICIOS_WEB]
    SRV --> S3[INTELIGENCIA_ARTIFICIAL]
    SRV --> S4[ACCESO_REMOTO]

    REC --> R1[CARPETAS_COMPARTIDAS]
    REC --> R2[IMPRESORAS]
    REC --> R3[APLICACIONES]

    style Root fill:#2c3e50,color:#fff,stroke:#333,stroke-width:2px
    style ADM,USR,EQP,SRV,GRP,REC fill:#3498db,color:#fff
    style U1,U2,U3,U4,U5,U6,U7,CLI,VPN,TST,S1,S2,S3,S4,R1,R2,R3 fill:#ecf0f1,color:#2c3e50
