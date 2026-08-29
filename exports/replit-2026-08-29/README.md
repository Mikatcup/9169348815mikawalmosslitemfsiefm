# MossLight Replit export

This directory contains a source snapshot exported from local commit 008b2112dbdbe7f7ef4f07bf001214670228d95d.

Snapshot SHA-256: 8ea6e2131185c815025e3954594236a19f738d587da4ea58321e7f8c9e588dad
Compressed bytes: 427243162
Parts: 14

Reconstruct on macOS/Linux:

    cat mosslight-snapshot.tar.gz.part-* > mosslight-snapshot.tar.gz
    printf "8ea6e2131185c815025e3954594236a19f738d587da4ea58321e7f8c9e588dad  mosslight-snapshot.tar.gz\n" | sha256sum -c -
    mkdir mosslight-source && tar -xzf mosslight-snapshot.tar.gz -C mosslight-source

Excluded from the source archive because they are oversized generated/media exports:
- exports/MossLight-source-bundle.zip
- attached_assets/ScreenRecording_08-28-2026_22-05-46_1_1787980420209.mov
- attached_assets/MossLight_IAP_Images_1024x1024.zip

The current project-plan exports are also included separately in this directory for direct browsing.
