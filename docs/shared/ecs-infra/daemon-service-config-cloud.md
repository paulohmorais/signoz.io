***Notes:***

- Depending on the choice of your region for SigNoz cloud, the OTLP endpoint
  will vary according to this table.

  | Region	| Endpoint |
  | --- | --- |
  | US | ingest.us.signoz.cloud:443 |
  | IN | ingest.in.signoz.cloud:443 |
  | EU | ingest.eu.signoz.cloud:443 |
- Replace `SIGNOZ_INGESTION_KEY` with the ingestion key of your SigNoz Cloud account
- You can find the **Ingestion Key** and **Region** in the SigNoz Cloud invitation email as well as the **Settings Tab** of your SigNoz Cloud UI.
- After successful set up, feel free to remove `logging` exporter if it gets too noisy.