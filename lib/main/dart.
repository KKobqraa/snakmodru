import 'dart:async';
import 'dart:math';
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'package:shared_preferences/shared_preferences.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await SystemChrome.setEnabledSystemUIMode(SystemUiMode.immersiveSticky);
  await SystemChrome.setPreferredOrientations([DeviceOrientation.portraitUp]);
  runApp(const SnakmodRUApp());
}

class SnakmodRUApp extends StatelessWidget {
  const SnakmodRUApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'SnakmodRU',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
        useMaterial3: true,
      ),
      debugShowCheckedModeBanner: false,
      home: const OnboardingPage(),
    );
  }
}

class PlayerInfo {
  final String fullName;
  final bool? served;
  final bool? wantServe;
  final String phone;

  const PlayerInfo({
    required this.fullName,
    required this.served,
    required this.wantServe,
    required this.phone,
  });
}

class OnboardingPage extends StatefulWidget {
  const OnboardingPage({super.key});

  @override
  State<OnboardingPage> createState() => _OnboardingPageState();
}

class _OnboardingPageState extends State<OnboardingPage> {
  final _formKey = GlobalKey<FormState>();
  final _nameCtrl = TextEditingController();
  final _phoneCtrl = TextEditingController(text: '+7 (*** ) ***-**-**');
  bool? _served;
  bool? _wantServe;

  @override
  void dispose() {
    _nameCtrl.dispose();
    _phoneCtrl.dispose();
    super.dispose();
  }

  void _startGame() {
    if (_formKey.currentState?.validate() ?? false) {
      final info = PlayerInfo(
        fullName: _nameCtrl.text.trim(),
        served: _served,
        wantServe: _served == false ? _wantServe : null,
        phone: _phoneCtrl.text.trim(),
      );
      Navigator.of(context).push(
        MaterialPageRoute(builder: (_) => GamePage(player: info)),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    final w = MediaQuery.of(context).size.width;
    return Scaffold(
      body: SafeArea(
        child: Center(
          child: ConstrainedBox(
            constraints: const BoxConstraints(maxWidth: 560),
            child: Padding(
              padding: const EdgeInsets.all(16),
              child: SingleChildScrollView(
                child: Column(
                  children: [
                    _FlagHeader(),
                    const SizedBox(height: 16),
                    Text('SnakmodRU',
                        style: Theme.of(context)
                            .textTheme
                            .headlineLarge
                            ?.copyWith(fontWeight: FontWeight.w700),
                        textAlign: TextAlign.center),
                    const SizedBox(height: 8),
                    Text(
                      'Триколорная змейка со смешной анкетой 😄\n'
                      'P.S. Ничего никуда не отправляется 🤫',
                      textAlign: TextAlign.center,
                    ),
                    const SizedBox(height: 24),
                    Card(
                      elevation: 2,
                      child: Padding(
                        padding: const EdgeInsets.all(16),
                        child: Form(
                          key: _formKey,
                          child: Column(
                            children: [
                              TextFormField(
                                controller: _nameCtrl,
                                decoration: const InputDecoration(
                                  labelText: 'ФИО',
                                  hintText: 'Иванов Иван Иванович (можно выдуманное)',
                                  prefixIcon: Icon(Icons.badge_outlined),
                                ),
                                validator: (v) =>
                                    (v == null || v.trim().isEmpty) ? 'Укажи хоть что-то :)' : null,
                              ),
                              const SizedBox(height: 12),
                              _YesNoTile(
                                title: 'Служил(а) в армии?',
                                value: _served,
                                onChanged: (val) => setState(() => _served = val),
                              ),
                              if (_served == false) ...[
                                const SizedBox(height: 8),
                                _YesNoTile(
                                  title: 'Если нет — хотел(а) бы?',
                                  value: _wantServe,
                                  onChanged: (val) => setState(() => _wantServe = val),
                                ),
                              ],
                              const SizedBox(height: 12),
                              TextFormField(
                                controller: _phoneCtrl,
                                decoration: const InputDecoration(
                                  labelText: 'Телефон (в шуточной форме)',
                                  hintText: '+7 (***) ***-**-**',
                                  prefixIcon: Icon(Icons.phone_iphone),
                                ),
                                validator: (v) => (v == null || v.trim().isEmpty)
                                    ? 'Можно даже написать «секретно» 😉'
                                    : null,
                              ),
                              const SizedBox(height: 16),
                              Row(
                                children: const [
                                  Icon(Icons.visibility_off_outlined, size: 18),
                                  SizedBox(width: 8),
                                  Expanded(
                                    child: Text(
                                      'Данные никуда не отправляются. Это чистая пародия.',
                                      style:
                                          TextStyle(fontSize: 12, color: Colors.black54),
                                    ),
                                  ),
                                ],
                              ),
                              const SizedBox(height: 16),
                              SizedBox(
                                width: double.infinity,
                                child: ElevatedButton.icon(
                                  icon: const Icon(Icons.play_arrow_rounded),
                                  label: const Text('Начать игру'),
                                  onPressed: _startGame,
                                ),
                              ),
                            ],
                          ),
                        ),
                      ),
                    ),
                    SizedBox(height: w < 360 ? 8 : 24),
                  ],
                ),
              ),
            ),
          ),
        ),
      ),
    );
  }
}

class _YesNoTile extends StatelessWidget {
  final String title;
  final bool? value;
  final ValueChanged<bool?> onChanged;
  const _YesNoTile({required this.title, required this.value, required this.onChanged});

  @override
  Widget build(BuildContext context) {
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Text(title, style: Theme.of(context).textTheme.titleMedium),
        const SizedBox(height: 6),
        SegmentedButton<bool>(
          segments: const [
            ButtonSegment<bool>(value: true, label: Text('Да')),
            ButtonSegment<bool>(value: false, label: Text('Нет')),
          ],
          selected: value == null ? <bool>{} : {value!},
          onSelectionChanged: (set) => onChanged(set.isEmpty ? null : set.first),
          emptySelectionAllowed: true,
        ),
      ],
    );
  }
}

class _FlagHeader extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ClipRRect(
      borderRadius: BorderRadius.circular(12),
      child: SizedBox(
        height: 16,
        child: Row(
          children: const [
            Expanded(child: ColoredBox(color: Colors.white)),
            Expanded(child: ColoredBox(color: Colors.blue)),
            Expanded(child: ColoredBox(color: Colors.red)),
          ],
        ),
      ),
    );
  }
}

class GamePage extends StatefulWidget {
  final PlayerInfo player;
  const GamePage({super.key, required this.player});

  @override
  State<GamePage> createState() => _GamePageState();
}

enum Direction { up, right, down, left }

class Cell {
  final int x;
  final int y;
  const Cell(this.x, this.y);

  @override
  bool operator ==(Object other) => other is Cell && other.x == x && other.y == y;
  @override
  int get hashCode => x.hashCode ^ (y.hashCode << 1);
}

class _GamePageState extends State<GamePage> {
  // Grid
  final int cols = 20;
  final int rows = 20;

  // Game state
  late List<Cell> snake;
  late Direction dir;
  Cell? food;
  Timer? timer;
  bool running = false;
  bool gameOver = false;
  int score = 0;
  int bestScore = 0;

  // Settings
  bool soundOn = true;
  bool hapticsOn = true;
  bool wrapWalls = false;
  double speed = 1.0; // 0.75..2.0
  static const _baseMs = 150;

  // UI state
  bool _overlayAgencies = true;
  bool _paused = false;

  @override
  void initState() {
    super.initState();
    _loadPrefs();
    _resetGame();
    Future.delayed(const Duration(milliseconds: 1200), () {
      if (mounted) setState(() => _overlayAgencies = false);
    });
  }

  @override
  void dispose() {
    timer?.cancel();
    super.dispose();
  }

  Future<void> _loadPrefs() async {
    final p = await SharedPreferences.getInstance();
    setState(() {
      bestScore = p.getInt('bestScore') ?? 0;
      soundOn = p.getBool('soundOn') ?? true;
      hapticsOn = p.getBool('hapticsOn') ?? true;
      wrapWalls = p.getBool('wrapWalls') ?? false;
      speed = p.getDouble('speed') ?? 1.0;
    });
  }

  Future<void> _savePrefs() async {
    final p = await SharedPreferences.getInstance();
    await p.setInt('bestScore', bestScore);
    await p.setBool('soundOn', soundOn);
    await p.setBool('hapticsOn', hapticsOn);
    await p.setBool('wrapWalls', wrapWalls);
    await p.setDouble('speed', speed);
  }

  void _startTimer() {
    timer?.cancel();
    timer = Timer.periodic(
      Duration(milliseconds: max(40, (_baseMs / speed).round())),
      (_) => _tick(),
    );
  }

  void _resetGame() {
    timer?.cancel();
    score = 0;
    gameOver = false;
    _paused = false;
    final startY = rows ~/ 2;
    snake = [
      const Cell(6, 10),
      const Cell(5, 10),
      const Cell(4, 10),
    ];
    dir = Direction.right;
    food = _randomFreeCell();
    running = true;
    _startTimer();
  }

  Cell _randomFreeCell() {
    final rng = Random();
    while (true) {
      final c = Cell(rng.nextInt(cols), rng.nextInt(rows));
      if (!snake.contains(c)) return c;
    }
  }

  void _tick() {
    if (!running || _paused || gameOver) return;
    setState(() {
      final head = snake.first;
      Cell next = _computeNext(head, dir);

      // collision with walls (if wrapWalls = false)
      if (!wrapWalls &&
          (next.x < 0 || next.x >= cols || next.y < 0 || next.y >= rows)) {
        _endGame();
        return;
      }

      // If wrap is on, bring inside field
      if (wrapWalls) {
        int nx = next.x, ny = next.y;
        if (nx < 0) nx = cols - 1;
        if (nx >= cols) nx = 0;
        if (ny < 0) ny = rows - 1;
        if (ny >= rows) ny = 0;
        next = Cell(nx, ny);
      }

      // collision with self
      if (snake.contains(next)) {
        _endGame();
        return;
      }

      snake = [next, ...snake];
      if (food != null && next == food) {
        score += 10;
        _fxEat();
        food = _randomFreeCell();
      } else {
        snake.removeLast();
      }
    });
  }

  Cell _computeNext(Cell c, Direction d) {
    switch (d) {
      case Direction.up:
        return Cell(c.x, c.y - 1);
      case Direction.right:
        return Cell(c.x + 1, c.y);
      case Direction.down:
        return Cell(c.x, c.y + 1);
      case Direction.left:
        return Cell(c.x - 1, c.y);
    }
  }

  Future<void> _endGame() async {
    running = false;
    gameOver = true;
    timer?.cancel();
    _fxCrash();
    bestScore = max(bestScore, score);
    await _savePrefs();

    Future.delayed(const Duration(milliseconds: 300), () {
      if (!mounted) return;
      showDialog(
        context: context,
        barrierDismissible: true,
        builder: (ctx) => AlertDialog(
          title: const Text('Игра окончена'),
          content: Text('Счёт: $score\nРекорд: $bestScore'),
          actions: [
            TextButton(
              onPressed: () {
                Navigator.of(ctx).pop();
                Navigator.of(context).pop();
              },
              child: const Text('В меню'),
            ),
            ElevatedButton(
              onPressed: () {
                Navigator.of(ctx).pop();
                _resetGame();
              },
              child: const Text('Сыграть ещё'),
            ),
          ],
        ),
      );
    });
  }

  void _changeDir(Direction newDir) {
    final isOpposite = (dir == Direction.up && newDir == Direction.down) ||
        (dir == Direction.down && newDir == Direction.up) ||
        (dir == Direction.left && newDir == Direction.right) ||
        (dir == Direction.right && newDir == Direction.left);
    if (!isOpposite) {
      setState(() => dir = newDir);
      if (hapticsOn) HapticFeedback.selectionClick();
    }
  }

  void _handleSwipe(DragUpdateDetails d) {
    if (d.delta.distance < 4) return;
    final dx = d.delta.dx;
    final dy = d.delta.dy;
    if (dx.abs() > dy.abs()) {
      _changeDir(dx > 0 ? Direction.right : Direction.left);
    } else {
      _changeDir(dy > 0 ? Direction.down : Direction.up);
    }
  }

  // Effects
  Future<void> _fxEat() async {
    if (soundOn) unawaited(SystemSound.play(SystemSoundType.click));
    if (hapticsOn) HapticFeedback.mediumImpact();
  }

  Future<void> _fxCrash() async {
    if (soundOn) unawaited(SystemSound.play(SystemSoundType.alert));
    if (hapticsOn) HapticFeedback.heavyImpact();
  }

  void _togglePause() {
    setState(() => _paused = !_paused);
    if (_paused) {
      if (hapticsOn) HapticFeedback.selectionClick();
    } else {
      if (hapticsOn) HapticFeedback.lightImpact();
    }
  }

  void _openSettings() async {
    await showModalBottomSheet(
      context: context,
      showDragHandle: true,
      builder: (ctx) {
        double tmpSpeed = speed;
        bool tmpSound = soundOn;
        bool tmpVibro = hapticsOn;
        bool tmpWrap = wrapWalls;
        return StatefulBuilder(builder: (ctx, setLocal) {
          return Padding(
            padding: const EdgeInsets.only(bottom: 16),
            child: ListView(
              shrinkWrap: true,
              children: [
                const ListTile(
                  title: Text('Настройки'),
                  subtitle: Text('Звук, вибро, стены и скорость'),
                ),
                SwitchListTile(
                  value: tmpSound,
                  onChanged: (v) => setLocal(() => tmpSound = v),
                  secondary: const Icon(Icons.volume_up),
                  title: const Text('Звук'),
                ),
                SwitchListTile(
                  value: tmpVibro,
                  onChanged: (v) => setLocal(() => tmpVibro = v),
                  secondary: const Icon(Icons.vibration),
                  title: const Text('Вибро'),
                ),
                SwitchListTile(
                  value: tmpWrap,
                  onChanged: (v) => setLocal(() => tmpWrap = v),
                  secondary: const Icon(Icons.border_style),
                  title: const Text('Сквозь стены (wrap)'),
                ),
                ListTile(
                  title: const Text('Скорость'),
                  subtitle: Slider(
                    value: tmpSpeed,
                    min: 0.75,
                    max: 2.0,
                    divisions: 5,
                    label: 'x${tmpSpeed.toStringAsFixed(2)}',
                    onChanged: (v) => setLocal(() => tmpSpeed = v),
                  ),
                  trailing: Text('x${tmpSpeed.toStringAsFixed(2)}'),
                ),
                const SizedBox(height: 6),
                Padding(
                  padding: const EdgeInsets.symmetric(horizontal: 16),
                  child: FilledButton.icon(
                    onPressed: () {
                      Navigator.of(ctx).pop();
                      setState(() {
                        soundOn = tmpSound;
                        hapticsOn = tmpVibro;
                        wrapWalls = tmpWrap;
                        speed = tmpSpeed;
                        _startTimer(); // обновить таймер под новую скорость
                      });
                      _savePrefs();
                    },
                    icon: const Icon(Icons.check),
                    label: const Text('Применить'),
                  ),
                ),
              ],
            ),
          );
        });
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    final media = MediaQuery.of(context);
    final size = media.size;

    // Layout math
    const topBarHeight = 92.0;
    final playable = Size(
      size.width,
      size.height - media.padding.top - media.padding.bottom - topBarHeight - 16,
    );
    final tile =
        (min(playable.width, playable.height) / max(rows, cols)).floorToDouble().clamp(8.0, 40.0);
    final boardW = tile * cols;
    final boardH = tile * rows;
    final boardLeft = (size.width - boardW) / 2;
    final boardTop = topBarHeight + (playable.height - boardH) / 2;

    return Scaffold(
      body: SafeArea(
        child: Stack(
          children: [
            // Tri-color background
            Positioned.fill(
              child: Column(
                children: const [
                  Expanded(child: ColoredBox(color: Colors.white)),
                  Expanded(child: ColoredBox(color: Colors.blue)),
                  Expanded(child: ColoredBox(color: Colors.red)),
                ],
              ),
            ),
            // Slight overlay for contrast
            Positioned.fill(child: Container(color: Colors.black.withOpacity(0.05))),

            // Top info bar
            Positioned(
              left: 8,
              right: 8,
              top: 8,
              child: _TopBar(
                player: widget.player,
                score: score,
                best: bestScore,
                paused: _paused,
                onPauseToggle: _togglePause,
                onRestart: _resetGame,
                onExit: () => Navigator.of(context).pop(),
                onOpenSettings: _openSettings,
              ),
            ),

            // Game board
            Positioned(
              left: boardLeft,
              top: boardTop,
              width: boardW,
              height: boardH,
              child: GestureDetector(
                onPanUpdate: _handleSwipe,
                onTap: _togglePause,
                child: DecoratedBox(
                  decoration: BoxDecoration(
                    color: Colors.transparent,
                    border: Border.all(color: Colors.black.withOpacity(0.2), width: 2),
                    borderRadius: BorderRadius.circular(8),
                  ),
                  child: CustomPaint(
                    painter: _SnakePainter(
                      rows: rows,
                      cols: cols,
                      tile: tile,
                      snake: snake,
                      food: food,
                    ),
                    child: const SizedBox.expand(),
                  ),
                ),
              ),
            ),

            // On-screen controls
            Positioned(
              left: 0,
              right: 0,
              bottom: 8,
              child: _Controls(
                onUp: () => _changeDir(Direction.up),
                onRight: () => _changeDir(Direction.right),
                onDown: () => _changeDir(Direction.down),
                onLeft: () => _changeDir(Direction.left),
              ),
            ),

            // Fun overlay: RKN + MoD joined
            if (_overlayAgencies)
              Positioned(
                top: 120,
                left: 16,
                right: 16,
                child: Center(
                  child: AnimatedOpacity(
                    opacity: _overlayAgencies ? 1 : 0,
                    duration: const Duration(milliseconds: 250),
                    child: Container(
                      padding: const EdgeInsets.symmetric(horizontal: 12, vertical: 10),
                      decoration: BoxDecoration(
                        color: Colors.black.withOpacity(0.75),
                        borderRadius: BorderRadius.circular(12),
                      ),
                      child: const Text(
                        'К игре подключились: РКН и Минобороны 👀',
                        style: TextStyle(color: Colors.white, fontSize: 14),
                      ),
                    ),
                  ),
                ),
              ),
          ],
        ),
      ),
    );
  }
}

class _TopBar extends StatelessWidget {
  final PlayerInfo player;
  final int score;
  final int best;
  final bool paused;
  final VoidCallback onPauseToggle;
  final VoidCallback onRestart;
  final VoidCallback onExit;
  final VoidCallback onOpenSettings;

  const _TopBar({
    required this.player,
    required this.score,
    required this.best,
    required this.paused,
    required this.onPauseToggle,
    required this.onRestart,
    required this.onExit,
    required this.onOpenSettings,
  });

  @override
  Widget build(BuildContext context) {
    final name = player.fullName.isEmpty ? 'Игрок' : player.fullName;
    final sub = _buildSubtitle();

    return Row(
      children: [
        Expanded(
          child: Card(
            elevation: 2,
            child: Padding(
              padding: const EdgeInsets.symmetric(horizontal: 12, vertical: 8),
              child: Row(
                children: [
                  const Icon(Icons.flag_circle_outlined, color: Colors.blue),
                  const SizedBox(width: 8),
                  Expanded(
                    child: Column(
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text('SnakmodRU — $name',
                            style: const TextStyle(fontWeight: FontWeight.w600)),
                        if (sub != null)
                          Text(sub, style: const TextStyle(fontSize: 12, color: Colors.black54)),
                      ],
                    ),
                  ),
                  const SizedBox(width: 8),
                  _ScoreBadge(label: 'Счёт', value: score),
                  const SizedBox(width: 8),
                  _ScoreBadge(label: 'Рекорд', value: best),
                  IconButton(
                    tooltip: 'Настройки',
                    onPressed: onOpenSettings,
                    icon: const Icon(Icons.tune_rounded),
                  ),
                  IconButton(
                    tooltip: paused ? 'Продолжить' : 'Пауза',
                    onPressed: onPauseToggle,
                    icon: Icon(paused ? Icons.play_arrow_rounded : Icons.pause_rounded),
                  ),
                  IconButton(
                    tooltip: 'Заново',
                    onPressed: onRestart,
                    icon: const Icon(Icons.replay_rounded),
                  ),
                  IconButton(
                    tooltip: 'В меню',
                    onPressed: onExit,
                    icon: const Icon(Icons.exit_to_app_rounded),
                  ),
                ],
              ),
            ),
          ),
        ),
      ],
    );
  }

  String? _buildSubtitle() {
    final served = player.served;
    final want = player.wantServe;
    if (served == null) return null;
    if (served == true) return 'Служба: проходил(а)';
    if (want == null) return 'Служба: не проходил(а)';
    return 'Служба: не проходил(а), хотел(а) бы: ${want ? 'да' : 'нет'}';
  }
}

class _ScoreBadge extends StatelessWidget {
  final String label;
  final int value;
  const _ScoreBadge({required this.label, required this.value});

  @override
  Widget build(BuildContext context) {
    return Chip(
      visualDensity: VisualDensity.compact,
      labelPadding: const EdgeInsets.symmetric(horizontal: 6),
      avatar: const Icon(Icons.star_border_rounded, size: 16),
      label: Text('$label: $value'),
    );
  }
}

class _Controls extends StatelessWidget {
  final VoidCallback onUp;
  final VoidCallback onRight;
  final VoidCallback onDown;
  final VoidCallback onLeft;

  const _Controls({
    required this.onUp,
    required this.onRight,
    required this.onDown,
    required this.onLeft,
  });

  @override
  Widget build(BuildContext context) {
    final bg = Colors.black.withOpacity(0.05);
    return Center(
      child: Container(
        padding: const EdgeInsets.all(8),
        decoration: BoxDecoration(
          color: bg,
          borderRadius: BorderRadius.circular(12),
        ),
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            _ArrowButton(icon: Icons.keyboard_arrow_up_rounded, onPressed: onUp),
            Row(
              mainAxisSize: MainAxisSize.min,
              children: [
                _ArrowButton(icon: Icons.keyboard_arrow_left_rounded, onPressed: onLeft),
                const SizedBox(width: 12),
                _ArrowButton(icon: Icons.keyboard_arrow_right_rounded, onPressed: onRight),
              ],
            ),
            _ArrowButton(icon: Icons.keyboard_arrow_down_rounded, onPressed: onDown),
          ],
        ),
      ),
    );
  }
}

class _ArrowButton extends StatelessWidget {
  final IconData icon;
  final VoidCallback onPressed;
  const _ArrowButton({required this.icon, required this.onPressed});

  @override
  Widget build(BuildContext context) {
    return Padding(
      padding: const EdgeInsets.all(4.0),
      child: Ink(
        decoration: const ShapeDecoration(
          color: Colors.white,
          shape: CircleBorder(),
        ),
        child: IconButton(icon: Icon(icon), onPressed: onPressed),
      ),
    );
  }
}

class _SnakePainter extends CustomPainter {
  final int rows;
  final int cols;
  final double tile;
  final List<Cell> snake;
  final Cell? food;

  _SnakePainter({
    required this.rows,
    required this.cols,
    required this.tile,
    required this.snake,
    required this.food,
  });

  final List<Color> tricolor = const [Colors.white, Colors.blueAccent, Colors.redAccent];

  @override
  void paint(Canvas canvas, Size size) {
    final fill = Paint()..style = PaintingStyle.fill;
    final stroke = Paint()
      ..style = PaintingStyle.stroke
      ..strokeWidth = 1.2
      ..color = Colors.black.withOpacity(0.7);

    // Light grid
    final gridPaint = Paint()
      ..style = PaintingStyle.stroke
      ..strokeWidth = 0.4
      ..color = Colors.black.withOpacity(0.08);
    for (int x = 0; x <= cols; x++) {
      final dx = x * tile;
      canvas.drawLine(Offset(dx, 0), Offset(dx, size.height), gridPaint);
    }
    for (int y = 0; y <= rows; y++) {
      final dy = y * tile;
      canvas.drawLine(Offset(0, dy), Offset(size.width, dy), gridPaint);
    }

    // Food
    if (food != null) {
      final cx = food!.x * tile + tile / 2;
      final cy = food!.y * tile + tile / 2;
      final foodPaint = Paint()..color = const Color(0xFFFFC107);
      canvas.drawCircle(Offset(cx, cy), tile * 0.36, foodPaint);
      canvas.drawCircle(Offset(cx - tile * 0.12, cy - tile * 0.12), tile * 0.08,
          Paint()..color = Colors.white.withOpacity(0.8));
    }

    // Snake segments with tricolor
    for (int i = 0; i < snake.length; i++) {
      final c = snake[i];
      final rect = Rect.fromLTWH(c.x * tile + 0.6, c.y * tile + 0.6, tile - 1.2, tile - 1.2);
      fill.color = tricolor[i % tricolor.length];
      final rrect = RRect.fromRectAndRadius(rect, Radius.circular(tile * 0.18));
      canvas.drawRRect(rrect, fill);
      canvas.drawRRect(rrect, stroke);
    }
  }

  @override
  bool shouldRepaint(covariant _SnakePainter oldDelegate) {
    return snake != oldDelegate.snake || food != oldDelegate.food || tile != oldDelegate.tile;
  }
}
