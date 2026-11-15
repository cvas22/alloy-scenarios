# Pinned by digest; renovate's docker manager tracks the base image.
FROM eclipse-temurin:25.0.5_8-jdk@sha256:b8705e936964c228dfce359e63dbf08f6c69cb380ed1d576b1f6b1064f15567d

WORKDIR /app
COPY Main.java .
# Precompile rather than using java's source-file launcher, so the flame
# graph frames are plain Main.* methods.
RUN javac Main.java

# DebugNonSafepoints makes the JIT keep debug info outside safepoints,
# which is what gives async-profiler accurate line-level frames. The small
# heap keeps the allocation churn cheap.
CMD ["java", "-Xmx256m", "-XX:+UnlockDiagnosticVMOptions", "-XX:+DebugNonSafepoints", "Main"]
