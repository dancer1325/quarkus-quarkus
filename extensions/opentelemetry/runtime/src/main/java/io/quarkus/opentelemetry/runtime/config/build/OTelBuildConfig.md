* `@ConfigMapping(prefix = "quarkus.otel")
  @ConfigRoot(phase = ConfigPhase.BUILD_AND_RUN_TIME_FIXED)
  public interface OTelBuildConfig {`
  * == build Time configuration /
    * contains ALL attributes -- related with -- classloading
      * Reason:🧠native image needs🧠
  * `@WithDefault("true")
    boolean enabled();`
    * TODO: If false, disable the OpenTelemetry usage at build time
    * All other Otel properties will be ignored at runtime.
    * Will pick up value from legacy property quarkus.opentelemetry.enabled
  * `@WithDefault("false")
    boolean simple();`
    * `true`
      * == 💡exporter sends telemetry data right away💡 
      * 👀disable batch processing👀
      * uses
        * spans & log records
    * `false`
      * default one
      * uses
        * serverless applications
    

    /**
     * Trace exporter configurations.
     */
    TracesBuildConfig traces();

    /**
     * Metrics exporter configurations.
     */
    MetricsBuildConfig metrics();

    /**
     * Logs exporter configurations.
     */
    LogsBuildConfig logs();

    /**
     * The propagators to be used. Use a comma-separated list for multiple propagators.
     * <p>
     * Has values from {@link PropagatorType} or the full qualified name of a class implementing
     * {@link io.opentelemetry.context.propagation.TextMapPropagator}.
     * <p>
     * Default is {@value PropagatorType.Constants#TRACE_CONTEXT},{@value PropagatorType.Constants#BAGGAGE} (W3C).
     */
    @WithDefault(TRACE_CONTEXT + "," + BAGGAGE)
    List<String> propagators();

    /**
     * Enable/disable instrumentation for specific technologies.
     */
    InstrumentBuildTimeConfig instrument();

    /**
     * Allows to export Quarkus security events as the OpenTelemetry Span events.
     */
    SecurityEvents securityEvents();

    /**
     * Quarkus security events exported as the OpenTelemetry Span events.
     */
    @ConfigGroup
    interface SecurityEvents {
        /**
         * Whether exporting of the security events is enabled.
         */
        @WithDefault("false")
        boolean enabled();

        /**
         * Selects security event types.
         */
        @WithDefault("ALL")
        List<SecurityEventType> eventTypes();

        /**
         * Security event type.
         */
        enum SecurityEventType {
            /**
             * All the security events.
             */
            ALL(SecurityEvent.class),
            /**
             * Authentication success event.
             */
            AUTHENTICATION_SUCCESS(AuthenticationSuccessEvent.class),
            /**
             * Authentication failure event.
             */
            AUTHENTICATION_FAILURE(AuthenticationFailureEvent.class),
            /**
             * Authorization success event.
             */
            AUTHORIZATION_SUCCESS(AuthorizationSuccessEvent.class),
            /**
             * Authorization failure event.
             */
            AUTHORIZATION_FAILURE(AuthorizationFailureEvent.class),
            /**
             * Any other security event. For example the OpenId Connect security event belongs here.
             */
            OTHER(SecurityEvent.class);

            private final Class<? extends SecurityEvent> observedType;

            SecurityEventType(Class<? extends SecurityEvent> observedType) {
                this.observedType = observedType;
            }

            public Class<? extends SecurityEvent> getObservedType() {
                return observedType;
            }
        }
    }
}
