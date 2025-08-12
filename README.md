# ATKATKw
https://drive.google.com/drive/folders/1z39DWkTDZCT91NT5DKJWxRdrrBdHyWsd?usp=sharing

import cv2
import os
import time
from datetime import datetime
import argparse
import logging

class VideoRecorder:
    def __init__(self, rtsp_url, segment_duration=300, filename_format="video_{timestamp}.avi"):
        self.rtsp_url = rtsp_url
        self.segment_duration = segment_duration  # in seconds
        self.filename_format = filename_format
        self.folder_name = self.create_video_folder()
        self.cap = cv2.VideoCapture(rtsp_url)
        self.setup_logging()

        if not self.cap.isOpened():
            raise RuntimeError("Failed to open RTSP stream.")

        self.fps = self.cap.get(cv2.CAP_PROP_FPS)
        if self.fps == 0 or self.fps is None:
            self.fps = 25  # fallback default FPS
        self.frame_width = int(self.cap.get(cv2.CAP_PROP_FRAME_WIDTH))
        self.frame_height = int(self.cap.get(cv2.CAP_PROP_FRAME_HEIGHT))

    def setup_logging(self):
        logging.basicConfig(
            filename='video_log.txt',
            level=logging.INFO,
            format='%(asctime)s - %(message)s'
        )

    def create_video_folder(self):
        now = datetime.now()
        folder_name = now.strftime("%Y%m%d-%H%M")
        if not os.path.exists(folder_name):
            os.makedirs(folder_name)
        return folder_name

    def get_new_writer(self):
        timestamp = datetime.now().strftime("%Y%m%d-%H%M%S")
        video_name = os.path.join(self.folder_name, self.filename_format.format(timestamp=timestamp))
        fourcc = cv2.VideoWriter_fourcc(*'XVID')
        out = cv2.VideoWriter(video_name, fourcc, self.fps, (self.frame_width, self.frame_height))
        logging.info(f"Started new video file: {video_name}")
        return out, time.time()

    def record(self):
        out, start_time = self.get_new_writer()

        while True:
            ret, frame = self.cap.read()
            if not ret:
                logging.error("Decoding error. Restarting capture...")
                self.cap.release()
                time.sleep(2)
                self.cap = cv2.VideoCapture(self.rtsp_url)
                if not self.cap.isOpened():
                    continue
                continue

            current_time = time.time()
            elapsed_time = current_time - start_time

            out.write(frame)

            if elapsed_time >= self.segment_duration:
                out.release()
                out, start_time = self.get_new_writer()

    def run(self):
        try:
            self.record()
        except KeyboardInterrupt:
            logging.info("Recording stopped by user.")
        finally:
            self.cap.release()
            cv2.destroyAllWindows()


if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="Record video from an RTSP feed")
    parser.add_argument("rtsp_url", type=str, help="RTSP URL of the camera")
    parser.add_argument("segment_duration", type=int, nargs="?", default=300, help="Video segment duration in seconds (default: 300)")
    args = parser.parse_args()

    recorder = VideoRecorder(args.rtsp_url, segment_duration=args.segment_duration)
    recorder.run()

