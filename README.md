class UniversalInformationMetric {
    constructor() { 
        this.name = 'RUIM Framework'; 
        this.version = '1.0.0'; 
        this.author = 'Jitendra Mandiol'; 
    }

    // Calculate statistical features from signal
    extractFeatures(signal) {
        if (!signal || signal.length === 0) {
            return { mean: 0, variance: 0, entropy: 0 };
        }
        
        // Mean
        const mean = signal.reduce((sum, val) => sum + val, 0) / signal.length;

        // Variance
        const variance = signal.reduce((sum, val) => sum + Math.pow(val - mean, 2), 0) / signal.length;

        // Simple entropy approximation
        const entropy = Math.log(variance + 1);

        return { mean, variance, entropy };
    }

    // Calculate UIM distance between two signals
    calculateDistance(signalA, signalB) {
        console.log("▲ RUIM Analysis Started...");

        // Extract features
        const featuresA = this.extractFeatures(signalA);
        const featuresB = this.extractFeatures(signalB);

        // Calculate differences
        const meanDiff = Math.abs(featuresA.mean - featuresB.mean);
        const varDiff = Math.abs(featuresA.variance - featuresB.variance);
        const entropyDiff = Math.abs(featuresA.entropy - featuresB.entropy);

        // Combined distance (0-1 scale, where 0 = identical)
        const totalDistance = (meanDiff + varDiff + entropyDiff) / 3;
        const similarity = Math.max(0, 1 - totalDistance);

        return {
            distance: totalDistance.toFixed(4),
            similarity: (similarity * 100).toFixed(1) + '%',
            features: {
                signalA: featuresA,
                signalB: featuresB
            },
            interpretation: this.interpretResult(similarity)
        };
    }

    // Interpret the similarity result
    interpretResult(similarity) {
        if (similarity > 0.8) return "VERY HIGH SIMILARITY - Strong correlation";
        if (similarity > 0.6) return "HIGH SIMILARITY - Significant relationship";
        if (similarity > 0.4) return "MODERATE SIMILARITY - Some correlation";
        if (similarity > 0.2) return "LOW SIMILARITY - Few common patterns";
        return "VERY LOW SIMILARITY - Different patterns";
    }

    // Run demo with sample data
    runDemo() {
        console.log("\n🚀 RUNNING RUIM DEMO\n");

        // Sample signals from different domains
        const brainSignal = [1.2, 1.5, 1.3, 1.8, 1.4, 1.6, 1.2, 1.7];
        const cosmicSignal = [1.1, 1.6, 1.2, 1.7, 1.5, 1.3, 1.8, 1.4];

        const result = this.calculateDistance(brainSignal, cosmicSignal);
        
        console.log("Results:", result);
        console.log("\n✅ RUIM Analysis Complete!");
        
        return result;
    }
}

// Usage example:
const ruim = new UniversalInformationMetric();
ruim.runDemo();module.exports = UniversalInformationMetric;
