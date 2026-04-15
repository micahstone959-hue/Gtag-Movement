using UnityEngine;

[RequireComponent(typeof(AudioSource))]
public class SimpleVoiceChanger : MonoBehaviour
{
    public float pitch = 1.3f;     // 1.0 = normal
    public float distortion = 0.2f;
    public float lowPass = 5000f;

    AudioSource src;
    AudioLowPassFilter lp;
    AudioDistortionFilter dist;

    void Start()
    {
        src = GetComponent<AudioSource>();
        lp = gameObject.AddComponent<AudioLowPassFilter>();
        dist = gameObject.AddComponent<AudioDistortionFilter>();

        lp.cutoffFrequency = lowPass;
        dist.distortionLevel = distortion;

        src.pitch = pitch;
        src.loop = true;
        src.mute = false;

        // Start mic
        src.clip = Microphone.Start(null, true, 10, 44100);
        while (!(Microphone.GetPosition(null) > 0)) { }
        src.Play();
    }
}
