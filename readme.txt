See the [resources] folder for notes and usage example
sample of use:

void DecodeMP3(const void * mp3_file, unsigned file_size)
{
	OpenMP3::Library openmp3;

	OpenMP3::Iterator itr(openmp3, (OpenMP3::UInt8*)mp3_file, file_size);


	float buffer[2][1152];

	std::vector <float> channels[2];

	bool mono = true;


	OpenMP3::Decoder decoder(openmp3);

	OpenMP3::Frame frame;

	while (itr.GetNext(frame))
	{
		OpenMP3::UInt nsamples = decoder.ProcessFrame(frame, buffer);

		for(int ch = 0; ch < 2; ++ch)
		{
			auto & channel = channels[ch];

			auto * data = buffer[ch];

			for (OpenMP3::UInt idx = 0; idx < nsamples; ++idx) channel.push_back(*data++);
		}

		mono = mono && (frame.GetMode() == OpenMP3::kModeMono);
	}

	OpenMP3::UInt samples = channels[0].size();
}
